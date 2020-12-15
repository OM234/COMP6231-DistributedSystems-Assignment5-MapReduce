<h1>Hadoop MapReduce Assignment</h1>
<h2>Author: Osman Momoh</h2>
<h2>COMP 6231: Distributed Systems</h2>

<p>
    This program uses the Apache Hadoop framework and MapReduce libraries to count number of unique words in one or more 
    input files (with the exception of commonly used stop words). 
</p>
<p>
     <a href="https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html">
        Here are instructions for setting up a single Hadoop node cluster
     </a>
</p>
<p>
     <a href="https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html">
        Here are general instructions for implementing a MapReduce Word Count program
     </a>
</p>

<h3> Instruction Steps </h3>
<p>
    1) Compile the src/main/java files using the Hadoop compiler <br>
    2) Create a jar of the compiled classes <br>
    3) Using the Hadoop file system, copy the /input folder to the Hadoop Distributed File System (HDFS) <br>
    4) Using the Hadoop terminal command, run the jar, specifying the main class 'WordCount', the input folder 'input'
    and the output folder 'output' <br>
    5) Using Hadoop cat command, view the output file results to get the word counts
</p>

<h3> Implementation </h3>

Overall, this program uses the map reduce paradigm to distribute the workload of counting the unique words to multiple nodes. 

<h4> Map Function</h4>

The map function is located in WordCount.java in the TokenizerMapper class. This class extends Mapper<KEYIN, VALUEIN, KEYOUT, VALUEOUT>.
The StringTokenizer takes the text files and creates tokens which are the space seperated strings in the text. The StringTokenizer
is then iterated. During iteration, the words are converted to lower case. If the word is not a stop word, the word is sent 
to the combiner/reducer with the value one (e.g. <word, one>).


<h4> Reducer Function</h4>

The reducer function is location is WordCount.java in the IntSumReducer class. This class extends Reducer<KEYIN, VALUEIN, KEYOUT, VALUEOUT>.
IntSumReducer receives key/value pairs in the form <KEY, Iterable<VALUE>>. The key for this program is a unique word in the word.
The value is an iterable of IntWritables (essentially one values). For example if the reducer receives, <word, Iterable<IntWritable>>,
and this Iterable contains 10 one IntWriteables, the reducer with sum the ones and return <word, 10>.

<h4> Stop Words</h4>

<a href="http://www.textfixer.com/resources/common-englishwords-with-contractions.txt">The list of stop words used can be found here.</a>
In StopWords.java, a public and static Set<String> called stopWords is used in the Map function to determine if a word is a stop word.
A set is used for O(1) lookup times.