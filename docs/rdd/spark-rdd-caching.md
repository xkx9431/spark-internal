== RDD Caching and Persistence

*Caching* or *persistence* are optimisation techniques for (iterative and interactive) Spark computations. They help saving interim partial results so they can be reused in subsequent stages. These interim results as RDDs are thus kept in memory (default) or more solid storages like disk and/or replicated.

RDDs can be *cached* using <<cache, cache>> operation. They can also be *persisted* using <<persist, persist>> operation.

The difference between `cache` and `persist` operations is purely syntactic. `cache` is a synonym of `persist` or `persist(MEMORY_ONLY)`, i.e. `cache` is merely `persist` with the default storage level `MEMORY_ONLY`.

NOTE: Due to the very small and purely syntactic difference between caching and persistence of RDDs the two terms are often used interchangeably and I will follow the "pattern" here.

RDDs can also be <<unpersist, unpersisted>> to remove RDD from a permanent storage like memory and/or disk.

=== [[cache]] Caching RDD -- `cache` Method

[source, scala]
----
cache(): this.type = persist()
----

`cache` is a synonym of <<persist, persist>> with storage:StorageLevel.md[`MEMORY_ONLY` storage level].

=== [[persist]] Persisting RDD -- `persist` Methods

[source, scala]
----
persist(): this.type
persist(newLevel: StorageLevel): this.type
----

`persist` marks a RDD for persistence using `newLevel` storage:StorageLevel.md[storage level].

You can only change the storage level once or `persist` reports an `UnsupportedOperationException`:

```
Cannot change storage level of an RDD after it was already assigned a level
```

NOTE: You can _pretend_ to change the storage level of an RDD with already-assigned storage level only if the storage level is the same as it is currently assigned.

If the RDD is marked as persistent the first time, the RDD is core:ContextCleaner.md#registerRDDForCleanup[registered to `ContextCleaner`] (if available) and SparkContext.md#persistRDD[`SparkContext`].

The internal `storageLevel` attribute is set to the input `newLevel` storage level.

=== [[unpersist]] Unpersisting RDDs (Clearing Blocks) -- `unpersist` Method

[source, scala]
----
unpersist(blocking: Boolean = true): this.type
----

When called, `unpersist` prints the following INFO message to the logs:

```
INFO [RddName]: Removing RDD [id] from persistence list
```

It then calls SparkContext.md#unpersist[SparkContext.unpersistRDD(id, blocking)] and sets storage:StorageLevel.md[`NONE` storage level] as the current storage level.
