# HadoopRDD

https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.rdd.HadoopRDD[HadoopRDD] is an RDD that provides core functionality for reading data stored in HDFS, a local file system (available on all nodes), or any Hadoop-supported file system URI using the older MapReduce API (https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/package-summary.html[org.apache.hadoop.mapred]).

HadoopRDD is created as a result of calling the following methods in SparkContext.md[]:

* `hadoopFile`
* `textFile` (the most often used in examples!)
* `sequenceFile`

Partitions are of type `HadoopPartition`.

When an HadoopRDD is computed, i.e. an action is called, you should see the INFO message `Input split:` in the logs.

```
scala> sc.textFile("README.md").count
...
15/10/10 18:03:21 INFO HadoopRDD: Input split: file:/Users/jacek/dev/oss/spark/README.md:0+1784
15/10/10 18:03:21 INFO HadoopRDD: Input split: file:/Users/jacek/dev/oss/spark/README.md:1784+1784
...
```

The following properties are set upon partition execution:

* *mapred.tip.id* - task id of this task's attempt
* *mapred.task.id* - task attempt's id
* *mapred.task.is.map* as `true`
* *mapred.task.partition* - split id
* *mapred.job.id*

Spark settings for `HadoopRDD`:

* *spark.hadoop.cloneConf* (default: `false`) - shouldCloneJobConf - should a Hadoop job configuration `JobConf` object be cloned before spawning a Hadoop job. Refer to https://issues.apache.org/jira/browse/SPARK-2546[[SPARK-2546\] Configuration object thread safety issue]. When `true`, you should see a DEBUG message `Cloning Hadoop Configuration`.

You can register callbacks on [TaskContext](../scheduler/TaskContext.md).

HadoopRDDs are not checkpointed. They do nothing when `checkpoint()` is called.

[CAUTION]
====
FIXME

* What are `InputMetrics`?
* What is `JobConf`?
* What are the InputSplits: `FileSplit` and `CombineFileSplit`? * What are `InputFormat` and `Configurable` subtypes?
* What's InputFormat's RecordReader? It creates a key and a value. What are they?
* What's Hadoop Split? input splits for Hadoop reads? See `InputFormat.getSplits`
====

=== [[getPreferredLocations]] `getPreferredLocations` Method

CAUTION: FIXME

=== [[getPartitions]] `getPartitions` Method

The number of partition for HadoopRDD, i.e. the return value of `getPartitions`, is calculated using `InputFormat.getSplits(jobConf, minPartitions)` where `minPartitions` is only a hint of how many partitions one may want at minimum. As a hint it does not mean the number of partitions will be exactly the number given.

For `SparkContext.textFile` the input format class is https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/TextInputFormat.html[org.apache.hadoop.mapred.TextInputFormat].

The https://hadoop.apache.org/docs/current/api/org/apache/hadoop/mapred/FileInputFormat.html[javadoc of org.apache.hadoop.mapred.FileInputFormat] says:

> FileInputFormat is the base class for all file-based InputFormats. This provides a generic implementation of getSplits(JobConf, int). Subclasses of FileInputFormat can also override the isSplitable(FileSystem, Path) method to ensure input-files are not split-up and are processed as a whole by Mappers.

TIP: You may find https://github.com/apache/hadoop/blob/trunk/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-core/src/main/java/org/apache/hadoop/mapred/FileInputFormat.java#L319[the sources of org.apache.hadoop.mapred.FileInputFormat.getSplits] enlightening.
