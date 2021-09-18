# Spark Properties and spark-defaults.conf Properties File

*Spark properties* are the means of tuning the execution environment of a Spark application.

The default Spark properties file is <<spark-defaults-conf, `$SPARK_HOME/conf/spark-defaults.conf`>> that could be overriden using `spark-submit` with the spark-submit.md#properties-file[--properties-file] command-line option.

.Environment Variables
[options="header",width="100%"]
|===
| Environment Variable | Default Value | Description
| `SPARK_CONF_DIR` | `$\{SPARK_HOME}/conf` | Spark's configuration directory (with `spark-defaults.conf`)
|===

TIP: Read the official documentation of Apache Spark on http://spark.apache.org/docs/latest/configuration.html[Spark Configuration].

=== [[spark-defaults-conf]] `spark-defaults.conf` -- Default Spark Properties File

`spark-defaults.conf` (under `SPARK_CONF_DIR` or `$SPARK_HOME/conf`) is the default properties file with the Spark properties of your Spark applications.

NOTE: `spark-defaults.conf` is loaded by spark-AbstractCommandBuilder.md#loadPropertiesFile[AbstractCommandBuilder's `loadPropertiesFile` internal method].

=== [[getDefaultPropertiesFile]] Calculating Path of Default Spark Properties -- `Utils.getDefaultPropertiesFile` method

[source, scala]
----
getDefaultPropertiesFile(env: Map[String, String] = sys.env): String
----

`getDefaultPropertiesFile` calculates the absolute path to `spark-defaults.conf` properties file that can be either in directory specified by `SPARK_CONF_DIR` environment variable or `$SPARK_HOME/conf` directory.

NOTE: `getDefaultPropertiesFile` is part of `private[spark]` `org.apache.spark.util.Utils` object.
