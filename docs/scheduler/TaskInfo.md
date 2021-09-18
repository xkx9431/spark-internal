== [[TaskInfo]] TaskInfo

`TaskInfo` is information about a running task attempt inside a scheduler:TaskSet.md[TaskSet].

`TaskInfo` is created when:

* scheduler:TaskSetManager.md#resourceOffer[`TaskSetManager` dequeues a task for execution (given resource offer)] (and records the task as running)

* TaskUIData does `dropInternalAndSQLAccumulables`

* JsonProtocol utility is used to spark-history-server:JsonProtocol.md#taskInfoFromJson[re-create a task details from JSON]

NOTE: Back then, at the commit 63051dd2bcc4bf09d413ff7cf89a37967edc33ba, when `TaskInfo` was first merged to Apache Spark on 07/06/12, `TaskInfo` was part of `spark.scheduler.mesos` package -- note "Mesos" in the name of the package that shows how much Spark and Mesos influenced each other at that time.

[[internal-registries]]
.TaskInfo's Internal Registries and Counters
[cols="1,2",options="header",width="100%"]
|===
| Name
| Description

| [[finishTime]] `finishTime`
| Time when `TaskInfo` was <<markFinished, marked as finished>>.

Used when...FIXME
|===

=== [[creating-instance]] Creating TaskInfo Instance

`TaskInfo` takes the following when created:

* [[taskId]] Task ID
* [[index]] Index of the task within its scheduler:TaskSet.md[TaskSet] that may not necessarily be the same as the ID of the RDD partition that the task is computing.
* [[attemptNumber]] Task attempt ID
* [[launchTime]] Time when the task was dequeued for execution
* [[executorId]] Executor that has been offered (as a resource) to run the task
* [[host]] Host of the <<executorId, executor>>
* [[taskLocality]] scheduler:TaskSchedulerImpl.md#TaskLocality[TaskLocality], i.e. locality preference of the task
* [[speculative]] Flag whether a task is speculative or not

`TaskInfo` initializes the <<internal-registries, internal registries and counters>>.

=== [[markFinished]] Marking Task As Finished (Successfully or Not) -- `markFinished` Method

[source, scala]
----
markFinished(state: TaskState, time: Long = System.currentTimeMillis): Unit
----

`markFinished` records the input `time` as <<finishTime, finishTime>>.

`markFinished` marks `TaskInfo` as <<failed, failed>> when the input `state` is `FAILED` or <<killed, killed>> for `state` being `KILLED`.

NOTE: `markFinished` is used when `TaskSetManager` is notified that a task has finished scheduler:TaskSetManager.md#handleSuccessfulTask[successfully] or scheduler:TaskSetManager.md#handleFailedTask[failed].
