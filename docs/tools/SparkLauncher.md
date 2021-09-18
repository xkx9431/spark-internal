# SparkLauncher

`SparkLauncher` is an interface to launch Spark applications programmatically, i.e. from a code (not spark-submit.md[spark-submit] directly). It uses a builder pattern to configure a Spark application and launch it as a child process using spark-submit.md[spark-submit].

`SparkLauncher` uses [SparkSubmitCommandBuilder](SparkSubmitCommandBuilder.md) to build the Spark command of a Spark application to launch.

## <span id="NO_RESOURCE"> spark-internal

`SparkLauncher` defines `spark-internal` (`NO_RESOURCE`) as a special value to inform Spark not to try to process the application resource (_primary resource_) as a regular file (but as an imaginary resource that cluster managers would know how to look up and submit for execution, e.g. Spark on YARN or [Spark on Kubernetes]({{ book.spark_k8s }})).

`spark-internal` special value is used when:

* `SparkSubmit` is requested to [prepareSubmitEnvironment](SparkSubmit.md#prepareSubmitEnvironment) and checks whether to add the [primaryResource](SparkSubmitArguments.md#primaryResource) as part of the following:
  * `--jar` (for Spark on YARN in `cluster` deploy mode)
  * `--primary-*` arguments and define the `--main-class` argument (for [Spark on Kubernetes]({{ book.spark_k8s }}) in `cluster` deploy mode with `KubernetesClientApplication` main class)
* `SparkSubmit` is requested to [check whether a resource is internal or not](SparkSubmit.md#isInternal)

## Other

.``SparkLauncher``'s Builder Methods to Set Up Invocation of Spark Application
[options="header",width="100%"]
|===
| Setter | Description
| `addAppArgs(String... args)` | Adds command line arguments for a Spark application.
| `addFile(String file)` | Adds a file to be submitted with a Spark application.
| `addJar(String jar)` | Adds a jar file to be submitted with the application.
| `addPyFile(String file)` | Adds a python file / zip / egg to be submitted with a Spark application.
| `addSparkArg(String arg)` | Adds a no-value argument to the Spark invocation.
| `addSparkArg(String name, String value)` | Adds an argument with a value to the Spark invocation. It recognizes known command-line arguments, i.e. `--master`, `--properties-file`, `--conf`, `--class`, `--jars`, `--files`, and `--py-files`.
| `directory(File dir)` | Sets the working directory of spark-submit.
| `redirectError()` | Redirects stderr to stdout.
| `redirectError(File errFile)` | Redirects error output to the specified `errFile` file.
| `redirectError(ProcessBuilder.Redirect to)` | Redirects error output to the specified `to` Redirect.
| `redirectOutput(File outFile)` | Redirects output to the specified `outFile` file.
| `redirectOutput(ProcessBuilder.Redirect to)` | Redirects standard output to the specified `to` Redirect.
| `redirectToLog(String loggerName)` | Sets all output to be logged and redirected to a logger with the specified name.
| `setAppName(String appName)` | Sets the name of an Spark application
| `setAppResource(String resource)` | Sets the main application resource, i.e. the location of a jar file for Scala/Java applications.
| `setConf(String key, String value)` | Sets a Spark property. Expects `key` starting with `spark.` prefix.
| `setDeployMode(String mode)` | Sets the deploy mode.
| `setJavaHome(String javaHome)` | Sets a custom `JAVA_HOME`.
| `setMainClass(String mainClass)` | Sets the main class.
| `setMaster(String master)` | Sets the master URL.
| `setPropertiesFile(String path)` | Sets the internal `propertiesFile`.

See spark-AbstractCommandBuilder.md#loadPropertiesFile[`loadPropertiesFile` Internal Method].
| `setSparkHome(String sparkHome)` | Sets a custom `SPARK_HOME`.
| `setVerbose(boolean verbose)` | Enables verbose reporting for SparkSubmit.
|===

After the invocation of a Spark application is set up, use `launch()` method to launch a sub-process that will start the configured Spark application. It is however recommended to use `startApplication` method instead.

[source, scala]
----
import org.apache.spark.launcher.SparkLauncher

val command = new SparkLauncher()
  .setAppResource("SparkPi")
  .setVerbose(true)

val appHandle = command.startApplication()
----
