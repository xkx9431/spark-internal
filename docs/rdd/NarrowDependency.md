# NarrowDependency

`NarrowDependency` is a base (abstract) Dependency.md[Dependency] with _narrow_ (limited) number of [partitions](Partition.md) of the parent RDD that are required to compute a partition of the child RDD.

NOTE: Narrow dependencies allow for pipelined execution.

.Concrete ``NarrowDependency``-ies
[cols="1,2",options="header",width="100%"]
|===
| Name | Description
| <<OneToOneDependency, OneToOneDependency>> |
| <<PruneDependency, PruneDependency>> |
| <<RangeDependency, RangeDependency>> |
|===

=== [[contract]] `NarrowDependency` Contract

`NarrowDependency` contract assumes that extensions implement `getParents` method.

[source, scala]
----
def getParents(partitionId: Int): Seq[Int]
----

`getParents` returns the partitions of the parent RDD that the input `partitionId` depends on.

=== [[OneToOneDependency]] OneToOneDependency

`OneToOneDependency` is a narrow dependency that represents a one-to-one dependency between partitions of the parent and child RDDs.

```
scala> val r1 = sc.parallelize(0 to 9)
r1: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[13] at parallelize at <console>:18

scala> val r3 = r1.map((_, 1))
r3: org.apache.spark.rdd.RDD[(Int, Int)] = MapPartitionsRDD[19] at map at <console>:20

scala> r3.dependencies
res32: Seq[org.apache.spark.Dependency[_]] = List(org.apache.spark.OneToOneDependency@7353a0fb)

scala> r3.toDebugString
res33: String =
(8) MapPartitionsRDD[19] at map at <console>:20 []
 |  ParallelCollectionRDD[13] at parallelize at <console>:18 []
```

=== [[PruneDependency]] PruneDependency

`PruneDependency` is a narrow dependency that represents a dependency between the `PartitionPruningRDD` and its parent RDD.

=== [[RangeDependency]] RangeDependency

`RangeDependency` is a narrow dependency that represents a one-to-one dependency between ranges of partitions in the parent and child RDDs.

It is used in `UnionRDD` for `SparkContext.union`, `RDD.union` transformation to list only a few.

```
scala> val r1 = sc.parallelize(0 to 9)
r1: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[13] at parallelize at <console>:18

scala> val r2 = sc.parallelize(10 to 19)
r2: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[14] at parallelize at <console>:18

scala> val unioned = sc.union(r1, r2)
unioned: org.apache.spark.rdd.RDD[Int] = UnionRDD[16] at union at <console>:22

scala> unioned.dependencies
res19: Seq[org.apache.spark.Dependency[_]] = ArrayBuffer(org.apache.spark.RangeDependency@28408ad7, org.apache.spark.RangeDependency@6e1d2e9f)

scala> unioned.toDebugString
res18: String =
(16) UnionRDD[16] at union at <console>:22 []
 |   ParallelCollectionRDD[13] at parallelize at <console>:18 []
 |   ParallelCollectionRDD[14] at parallelize at <console>:18 []
```
