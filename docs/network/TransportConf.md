# TransportConf

TransportConf is a class for the transport-related network configuration for modules, e.g. [ExternalShuffleService](../external-shuffle-service/ExternalShuffleService.md) or spark-on-yarn:spark-yarn-YarnShuffleService.md[YarnShuffleService].

TransportConf exposes methods to access settings for a single module as <<spark.module.prefix, spark.module.prefix>> or <<general-settings, general network-related settings>>.

== [[spark.module.prefix]] spark.module.prefix Settings

The settings can be in the form of *spark.[module].[prefix]* with the following prefixes:

* `io.mode` (default: `NIO`) -- the IO mode: `nio` or `epoll`.

* `io.preferDirectBufs` (default: `true`) -- a flag to control whether Spark prefers allocating off-heap byte buffers within Netty (`true`) or not (`false`).

* `io.connectionTimeout` (default: rpc:index.md#spark.network.timeout[spark.network.timeout] or `120s`) -- the connection timeout in milliseconds.

* `io.backLog` (default: `-1` for no backlog) -- the requested maximum length of the queue of incoming connections.

* `io.numConnectionsPerPeer` (default: `1`) -- the number of concurrent connections between two nodes for fetching data.

* `io.serverThreads` (default: `0` i.e. 2x#cores) -- the number of threads used in the server thread pool.

* `io.clientThreads` (default: `0` i.e. 2x#cores) -- the number of threads used in the client thread pool.

* `io.receiveBuffer` (default: `-1`) -- the receive buffer size (SO_RCVBUF).

* `io.sendBuffer` (default: `-1`) -- the send buffer size (SO_SNDBUF).

* `sasl.timeout` (default: `30s`) -- the timeout (in milliseconds) for a single round trip of SASL token exchange.

* [[io.maxRetries]] `io.maxRetries` (default: `3`) -- the maximum number of times Spark will try IO exceptions (such as connection timeouts) per request. If set to `0`, Spark will not do any retries.

* `io.retryWait` (default: `5s`) -- the time (in milliseconds) that Spark will wait in order to perform a retry after an `IOException`. Only relevant if `io.maxRetries` > 0.

* `io.lazyFD` (default: `true`) -- controls whether to initialize `FileDescriptor` lazily (`true`) or not (`false`). If `true`, file descriptors are created only when data is going to be transferred. This can reduce the number of open files.

== [[general-settings]] General Network-Related Settings

=== [[spark.storage.memoryMapThreshold]] spark.storage.memoryMapThreshold

`spark.storage.memoryMapThreshold` (default: `2m`) is the minimum size of a block that we should start using memory map rather than reading in through normal IO operations.

This prevents Spark from memory mapping very small blocks. In general, memory mapping has high overhead for blocks close to or below the page size of the OS.

=== [[spark.network.sasl.maxEncryptedBlockSize]] spark.network.sasl.maxEncryptedBlockSize

`spark.network.sasl.maxEncryptedBlockSize` (default: `64k`) is the maximum number of bytes to be encrypted at a time when SASL encryption is enabled.

=== [[spark.network.sasl.serverAlwaysEncrypt]] spark.network.sasl.serverAlwaysEncrypt

`spark.network.sasl.serverAlwaysEncrypt` (default: `false`) controls whether the server should enforce encryption on SASL-authenticated connections (`true`) or not (`false`).
