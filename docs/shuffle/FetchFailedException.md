== [[FetchFailedException]] FetchFailedException

`FetchFailedException` exception executor:TaskRunner.md#run-FetchFailedException[may be thrown when a task runs] (and storage:ShuffleBlockFetcherIterator.md#throwFetchFailedException[`ShuffleBlockFetcherIterator` did not manage to fetch shuffle blocks]).

`FetchFailedException` contains the following:

* the unique identifier for a storage:BlockManager.md[BlockManager] (as storage:BlockManagerId.md[])
* `shuffleId`
* `mapId`
* `reduceId`
* A short exception `message`
* `cause` - the root `Throwable` object

When `FetchFailedException` is reported, executor:TaskRunner.md#run-FetchFailedException[`TaskRunner` catches it and notifies `ExecutorBackend`] (with `TaskState.FAILED` task state).

The root `cause` of the `FetchFailedException` is usually because the executor:Executor.md[] (with the storage:BlockManager.md[BlockManager] for the shuffle blocks) is lost (i.e. no longer available) due to:

1. `OutOfMemoryError` could be thrown (aka _OOMed_) or some other unhandled exception.
2. The cluster manager that manages the workers with the executors of your Spark application, e.g. YARN, enforces the container memory limits and eventually decided to kill the executor due to excessive memory usage.

You should review the logs of the Spark application using webui:index.md[web UI], spark-history-server:index.md[Spark History Server] or cluster-specific tools like https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/YarnCommands.html#logs[yarn logs -applicationId] for Hadoop YARN.

A solution is usually to tune the memory of your Spark application.

CAUTION: FIXME Image with the call to ExecutorBackend.

=== [[toTaskFailedReason]] `toTaskFailedReason` Method

CAUTION: FIXME
