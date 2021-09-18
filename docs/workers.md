== Workers

*Workers* (aka *slaves*) are running Spark instances where executors live to execute tasks. They are the compute nodes in Spark.

CAUTION: FIXME Are workers perhaps part of Spark Standalone only?

CAUTION: FIXME How many executors are spawned per worker?

A worker receives serialized tasks that it runs in a thread pool.

It hosts a local storage:BlockManager.md[Block Manager] that serves blocks to other workers in a Spark cluster. Workers communicate among themselves using their Block Manager instances.

CAUTION: FIXME Diagram of a driver with workers as boxes.

Explain task execution in Spark and understand Spark’s underlying execution model.

New vocabulary often faced in Spark UI

SparkContext.md[When you create SparkContext], each worker starts an executor. This is a separate process (JVM), and it loads your jar, too. The executors connect back to your driver program. Now the driver can send them commands, like `flatMap`, `map` and `reduceByKey`. When the driver quits, the executors shut down.

A new process is not started for each step. A new process is started on each worker when the SparkContext is constructed.

The executor deserializes the command (this is possible because it has loaded your jar), and executes it on a partition.

Shortly speaking, an application in Spark is executed in three steps:

1. Create RDD graph, i.e. DAG (directed acyclic graph) of RDDs to represent entire computation.
2. Create stage graph, i.e. a DAG of stages that is a logical execution plan based on the RDD graph. Stages are created by breaking the RDD graph at shuffle boundaries.
3. Based on the plan, schedule and execute tasks on workers.

exercises/spark-examples-wordcount-spark-shell.md[In the WordCount example], the RDD graph is as follows:

file -> lines -> words -> per-word count -> global word count -> output

Based on this graph, two stages are created. The *stage* creation rule is based on the idea of *pipelining* as many rdd:index.md[narrow transformations] as possible. RDD operations with "narrow" dependencies, like `map()` and `filter()`, are pipelined together into one set of tasks in each stage.

In the end, every stage will only have shuffle dependencies on other stages, and may compute multiple operations inside it.

In the WordCount example, the narrow transformation finishes at per-word count. Therefore, you get two stages:

* file -> lines -> words -> per-word count
* global word count -> output

Once stages are defined, Spark will generate scheduler:Task.md[tasks] from scheduler:Stage.md[stages]. The first stage will create scheduler:ShuffleMapTask.md[ShuffleMapTask]s with the last stage creating scheduler:ResultTask.md[ResultTask]s because in the last stage, one action operation is included to produce results.

The number of tasks to be generated depends on how your files are distributed. Suppose that you have 3 three different files in three different nodes, the first stage will generate 3 tasks: one task per partition.

Therefore, you should not map your steps to tasks directly. A task belongs to a stage, and is related to a partition.

The number of tasks being generated in each stage will be equal to the number of partitions.

=== [[Cleanup]] Cleanup

CAUTION: FIXME

=== [[settings]] Settings

* `spark.worker.cleanup.enabled` (default: `false`) <<Cleanup, Cleanup>> enabled.
