---
tags:
  - DeveloperApi
---

# StatsReportListener &mdash; Logging Summary Statistics

`org.apache.spark.scheduler.StatsReportListener` (see https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.scheduler.StatsReportListener[the listener's scaladoc]) is a SparkListener.md[] that logs summary statistics when each stage completes.

`StatsReportListener` listens to [SparkListenerTaskEnd](SparkListenerTaskEnd.md) and [SparkListenerStageCompleted](SparkListener.md#SparkListenerStageCompleted) events and prints them out at `INFO` logging level.

[TIP]
====
Enable `INFO` logging level for `org.apache.spark.scheduler.StatsReportListener` logger to see Spark events.

Add the following line to `conf/log4j.properties`:

```
log4j.logger.org.apache.spark.scheduler.StatsReportListener=INFO
```

Refer to spark-logging.md[Logging].
====

=== [[onStageCompleted]] Intercepting Stage Completed Events -- `onStageCompleted` Callback

CAUTION: FIXME

=== [[example]] Example

```
$ ./bin/spark-shell -c spark.extraListeners=org.apache.spark.scheduler.StatsReportListener
...
INFO SparkContext: Registered listener org.apache.spark.scheduler.StatsReportListener
...

scala> spark.read.text("README.md").count
...
INFO StatsReportListener: Finished stage: Stage(0, 0); Name: 'count at <console>:24'; Status: succeeded; numTasks: 1; Took: 212 msec
INFO StatsReportListener: task runtime:(count: 1, mean: 198.000000, stdev: 0.000000, max: 198.000000, min: 198.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	198.0 ms	198.0 ms	198.0 ms	198.0 ms	198.0 ms	198.0 ms	198.0 ms	198.0 ms	198.0 ms
INFO StatsReportListener: shuffle bytes written:(count: 1, mean: 59.000000, stdev: 0.000000, max: 59.000000, min: 59.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	59.0 B	59.0 B	59.0 B	59.0 B	59.0 B	59.0 B	59.0 B	59.0 B	59.0 B
INFO StatsReportListener: fetch wait time:(count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms
INFO StatsReportListener: remote bytes read:(count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B
INFO StatsReportListener: task result size:(count: 1, mean: 1885.000000, stdev: 0.000000, max: 1885.000000, min: 1885.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	1885.0 B	1885.0 B	1885.0 B	1885.0 B	1885.0 B	1885.0 B	1885.0 B	1885.0 B	1885.0 B
INFO StatsReportListener: executor (non-fetch) time pct: (count: 1, mean: 73.737374, stdev: 0.000000, max: 73.737374, min: 73.737374)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	74 %	74 %	74 %	74 %	74 %	74 %	74 %	74 %	74 %
INFO StatsReportListener: fetch wait time pct: (count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %
INFO StatsReportListener: other time pct: (count: 1, mean: 26.262626, stdev: 0.000000, max: 26.262626, min: 26.262626)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	26 %	26 %	26 %	26 %	26 %	26 %	26 %	26 %	26 %
INFO StatsReportListener: Finished stage: Stage(1, 0); Name: 'count at <console>:24'; Status: succeeded; numTasks: 1; Took: 34 msec
INFO StatsReportListener: task runtime:(count: 1, mean: 33.000000, stdev: 0.000000, max: 33.000000, min: 33.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	33.0 ms	33.0 ms	33.0 ms	33.0 ms	33.0 ms	33.0 ms	33.0 ms	33.0 ms	33.0 ms
INFO StatsReportListener: shuffle bytes written:(count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B
INFO StatsReportListener: fetch wait time:(count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms	0.0 ms
INFO StatsReportListener: remote bytes read:(count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B	0.0 B
INFO StatsReportListener: task result size:(count: 1, mean: 1960.000000, stdev: 0.000000, max: 1960.000000, min: 1960.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	1960.0 B	1960.0 B	1960.0 B	1960.0 B	1960.0 B	1960.0 B	1960.0 B	1960.0 B	1960.0 B
INFO StatsReportListener: executor (non-fetch) time pct: (count: 1, mean: 75.757576, stdev: 0.000000, max: 75.757576, min: 75.757576)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	76 %	76 %	76 %	76 %	76 %	76 %	76 %	76 %	76 %
INFO StatsReportListener: fetch wait time pct: (count: 1, mean: 0.000000, stdev: 0.000000, max: 0.000000, min: 0.000000)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %	 0 %
INFO StatsReportListener: other time pct: (count: 1, mean: 24.242424, stdev: 0.000000, max: 24.242424, min: 24.242424)
INFO StatsReportListener: 	0%	5%	10%	25%	50%	75%	90%	95%	100%
INFO StatsReportListener: 	24 %	24 %	24 %	24 %	24 %	24 %	24 %	24 %	24 %
res0: Long = 99
```
