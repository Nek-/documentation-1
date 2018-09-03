---
outputFileName: index.html
---

# Command Line Arguments
Event Store supports many configuration options. There are three distinct ways to set any parameter, all with their own advantages and disadvantages.

-   Via command line
-   Environment variables
-   YAML files.

## Command Line

To pass a configuration value over the command line you add the configuration to the line executing Event Store e.g.

```powershell
EventStore.ClusterNode.exe --log ~/logs
```

While command line arguments tend to be useful in development scenarios, they are often not the preferred way to handle configuration in a production system.

## Environment Variables

You can set all arguments can also as environment variables. This mechanism is often used in UNIX based systems.

<!-- TODO: Example? -->

## YAML Files

The last way you can set arguments is to put them into one or more configuration files. To tell Event Store to use a configuration file you pass the files on the command line with `--config=filename`. The basic format of the YAML configuration file is as follows:

```yaml
---
Log: "/home/Ouro/logs"
IntHttpPort: 2111
---
```

> [!NOTE]
> You need to use the three dashes and spacing in your YAML file.

Files can be better for large installations as you can centrally distribute and manage them, or generate them by a configuration management system such as Puppet.

The order of precedence between the multiple configuration sources is also important as you could feasibly set them in multiple ways. The command line is the highest priority followed by environment variables. Files are the lowest precedence and are processed in the order given on the command line. When starting Event Store the major parameters are listed in the log (including what set them).

```text
ES VERSION:               1.0.3 (dev/5488fa34228e283bad985966b700e1fc48a0780a, Tue, 2 Jul 2013 12:39:02 +0300)
OS:                       Linux (Unix 3.2.0.27)
RUNTIME:                  3.0.10 (master/1166156 Tue Apr 23 14:56:42 EEST 2013) (EventStore build) (64-bit)
GC:                       2 GENERATIONS
LOGS:                     /home/greg/foo

SHOW HELP:                False (<DEFAULT>)
SHOW VERSION:             False (<DEFAULT>)
LOGS DIR:                 /home/greg/foo (--logsdir from command line)
CONFIGS:                  <empty> (<DEFAULT>)
DEFINES:                  <empty> (<DEFAULT>)
IP:                       127.0.0.1 (<DEFAULT>)
TCP PORT:                 1113 (<DEFAULT>)
SECURE TCP PORT:          0 (<DEFAULT>)
HTTP PORT:                2113 (<DEFAULT>)
STATS PERIOD SEC:         30 (<DEFAULT>)
CACHED CHUNKS:            -1 (<DEFAULT>)
CHUNKS CACHE SIZE:        536871424 (<DEFAULT>)
DB PATH:                  /tmp/foo (--db from command line)
SKIP DB VERIFY:           False (<DEFAULT>)
RUN PROJECTIONS:          True (<DEFAULT>)
PROJECTION THREADS:       3 (<DEFAULT>)
WORKER THREADS:           5 (<DEFAULT>)
HTTP PREFIXES:            <empty> (<DEFAULT>)
ENABLE TRUSTED AUTH:      False (<DEFAULT>)
CERTIFICATE STORE:        <empty> (<DEFAULT>)
CERTIFICATE NAME:         <empty> (<DEFAULT>)
CERTIFICATE FILE:         <empty> (<DEFAULT>)
CERTIFICATE PASSWORD:     <empty> (<DEFAULT>)
PREPARE TIMEOUT MS:       2000 (<DEFAULT>)
COMMIT TIMEOUT MS:        2000 (<DEFAULT>)
```

> [!NOTE]
> User projections are not enabled by default, but the projections engine is used internally for account management. If you want to run user projections, you have to start Event Store using the `--run-projections=all` command line parameter.

Event Store supports the following parameters:

### Application Options

| Parameter                                                             | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                     | Description                                                                                                       |
| --------------------------------------------------------------------- | ---------------------------------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| -Help<br/>--help=VALUE<br/>                                           | HELP                                           | Help                     | Show help. (Default: False)                                                                                       |
| -Version<br/>--version=VALUE<br/>                                     | VERSION                                        | Version                  | Show version. (Default: False)                                                                                    |
| -Log<br/>--log=VALUE<br/>                                             | LOG                                            | Log                      | Path where to keep log files. (Default: [See default directories](default-directories.md)                        |
| -Config<br/>--config=VALUE<br/>                                       | CONFIG                                         | Config                   | Configuration files.                                                                                              |
| -Defines<br/>--defines=VALUE<br/>                                     | DEFINES                                        | Defines                  | Run-time conditionals. (Default: n/a)                                                                             |
| -WhatIf<br/>--what-if=VALUE<br/>                                      | WHAT_IF                                        | WhatIf                   | Print effective configuration to console and then exit. (Default: False)                                          |
| -StartStandardProjections<br/>--start-standard-projections=VALUE<br/> | START_STANDARD_PROJECTIONS                     | StartStandardProjections | Start the built in system projections. (Default: False)                                                           |
| -DisableHTTPCaching<br/>--disable-http-caching=VALUE<br/>             | DISABLE_HTTP_CACHING                           | DisableHTTPCaching       | Disable HTTP caching. (Default: False)                                                                            |
| -MonoMinThreadpoolSize<br/>--mono-min-threadpool-size=VALUE<br/>      | MONO_MIN_THREADPOOL_SIZE                       | MonoMinThreadpoolSize    | Minimum number of worker threads when running under mono. Set to 0 to leave machine defaults. (Default: 10)       |
| -Force<br/>--force=VALUE<br/>                                         | FORCE                                          | Force                    | Force the Event Store to run in possibly harmful environments such as with Boehm GC. (Default: False)             |
| -StatsPeriodSec<br/>--stats-period-sec=VALUE<br/>                     | STATS_PERIOD_SEC                               | StatsPeriodSec           | The number of seconds between statistics gathers. (Default: 30)                                                   |
| -WorkerThreads<br/>--worker-threads=VALUE<br/>                        | WORKER_THREADS                                 | WorkerThreads            | The number of threads to use for pool of worker services. (Default: 5)                                            |
| -EnableHistograms<br/>--enable-histograms=VALUE<br/>                  | ENABLE_HISTOGRAMS                              | EnableHistograms         | Enables the tracking of various histograms in the backend, typically only used for debugging etc (Default: False) |
| -LogHttpRequests<br/>--log-http-requests=VALUE<br/>                   | LOG_HTTP_REQUESTS                              | LogHttpRequests          | Log Http Requests and Responses before processing them. (Default: False)                                          |

### Authentication Options

| Parameter                                                    | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                 | Description                                                                      |
| ------------------------------------------------------------ | ---------------------------------------------- | -------------------- | -------------------------------------------------------------------------------- |
| -AuthenticationType<br/>--authentication-type=VALUE<br/>     | AUTHENTICATION_TYPE                            | AuthenticationType   | The type of authentication to use. (Default: internal)                           |
| -AuthenticationConfig<br/>--authentication-config=VALUE<br/> | AUTHENTICATION_CONFIG                          | AuthenticationConfig | Path to the configuration file for authentication configuration (if applicable). |

### Certificate Options

| Parameter                                                             | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                     | Description                             |
| --------------------------------------------------------------------- | ---------------------------------------------- | ------------------------ | --------------------------------------- |
| -CertificateStoreLocation<br/>--certificate-store-location=VALUE<br/> | CERTIFICATE_STORE_LOCATION                     | CertificateStoreLocation | The certificate store location name.    |
| -CertificateStoreName<br/>--certificate-store-name=VALUE<br/>         | CERTIFICATE_STORE_NAME                         | CertificateStoreName     | The certificate store name.             |
| -CertificateSubjectName<br/>--certificate-subject-name=VALUE<br/>     | CERTIFICATE_SUBJECT_NAME                       | CertificateSubjectName   | The certificate subject name.           |
| -CertificateThumbprint<br/>--certificate-thumbprint=VALUE<br/>        | CERTIFICATE_THUMBPRINT                         | CertificateThumbprint    | The certificate fingerprint/thumbprint. |
| -CertificateFile<br/>--certificate-file=VALUE<br/>                    | CERTIFICATE_FILE                               | CertificateFile          | The path to certificate file.           |
| -CertificatePassword<br/>--certificate-password=VALUE<br/>            | CERTIFICATE_PASSWORD                           | CertificatePassword      | The password to certificate in file.    |

### Cluster Options

| Parameter                                                                | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                      | Description                                                                                             |
| ------------------------------------------------------------------------ | ---------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------- |
| -ClusterSize<br/>--cluster-size=VALUE<br/>                               | CLUSTER_SIZE                                   | ClusterSize               | The number of nodes in the cluster. (Default: 1)                                                        |
| -NodePriority<br/>--node-priority=VALUE<br/>                             | NODE_PRIORITY                                  | NodePriority              | The node priority used during master election (Default: 0)                                              |
| -CommitCount<br/>--commit-count=VALUE<br/>                               | COMMIT_COUNT                                   | CommitCount               | The number of nodes which must acknowledge commits before acknowledging to a client. (Default: -1)      |
| -PrepareCount<br/>--prepare-count=VALUE<br/>                             | PREPARE_COUNT                                  | PrepareCount              | The number of nodes which must acknowledge prepares. (Default: -1)                                      |
| -DiscoverViaDns<br/>--discover-via-dns=VALUE<br/>                        | DISCOVER_VIA_DNS                               | DiscoverViaDns            | Whether to use DNS lookup to discover other cluster nodes. (Default: True)                              |
| -ClusterDns<br/>--cluster-dns=VALUE<br/>                                 | CLUSTER_DNS                                    | ClusterDns                | DNS name from which other nodes can be discovered. (Default: fake.dns)                                  |
| -ClusterGossipPort<br/>--cluster-gossip-port=VALUE<br/>                  | CLUSTER_GOSSIP_PORT                            | ClusterGossipPort         | The port on which cluster nodes' managers are running. (Default: 30777)                                 |
| -GossipSeed<br/>--gossip-seed=VALUE<br/>                                 | GOSSIP_SEED                                    | GossipSeed                | Endpoints for other cluster nodes from which to seed gossip (Default: n/a)                              |
| -GossipIntervalMs<br/>--gossip-interval-ms=VALUE<br/>                    | GOSSIP_INTERVAL_MS                             | GossipIntervalMs          | The interval, in ms, nodes should try to gossip with each other. (Default: 1000)                        |
| -GossipAllowedDifferenceMs<br/>--gossip-allowed-difference-ms=VALUE<br/> | GOSSIP_ALLOWED_DIFFERENCE_MS                   | GossipAllowedDifferenceMs | The amount of drift, in ms, between clocks on nodes allowed before gossip is rejected. (Default: 60000) |
| -GossipTimeoutMs<br/>--gossip-timeout-ms=VALUE<br/>                      | GOSSIP_TIMEOUT_MS                              | GossipTimeoutMs           | The timeout, in ms, on gossip to another node. (Default: 500)                                           |

### Database Options

| Parameter                                                               | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                     | Description                                                                                                    |
| ----------------------------------------------------------------------- | ---------------------------------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| -MinFlushDelayMs<br/>--min-flush-delay-ms=VALUE<br/>                    | MIN_FLUSH_DELAY_MS                             | MinFlushDelayMs          | The minimum flush delay in milliseconds. (Default: 2)                                                          |
| -DisableScavengeMerging<br/>--disable-scavenge-merging=VALUE<br/>       | DISABLE_SCAVENGE_MERGING                       | DisableScavengeMerging   | Disables the merging of chunks when scavenge is running (Default: False)                                       |
| -ScavengeHistoryMaxAge<br/>--scavenge-history-max-age=VALUE<br/>        | SCAVENGE_HISTORY_MAX_AGE                       | ScavengeHistoryMaxAge    | The number of days to keep scavenge history (Default: 30)                                                      |
| -CachedChunks<br/>--cached-chunks=VALUE<br/>                            | CACHED_CHUNKS                                  | CachedChunks             | The number of chunks to cache in unmanaged memory. (Default: -1)                                               |
| -ChunkSize<br/>--chunk-size=VALUE<br/>                            | CHUNK_SIZE                                  | ChunkSize             | Sets the chunk size. (Default: 256MB†)                                               |
| -ReaderThreadsCount<br/>--reader-threads-count=VALUE<br/>               | READER_THREADS_COUNT                           | ReaderThreadsCount       | The number of reader threads to use for processing reads. (Default: 4)                                         |
| -ChunksCacheSize<br/>--chunks-cache-size=VALUE<br/>                     | CHUNKS_CACHE_SIZE                              | ChunksCacheSize          | The amount of unmanaged memory to use for caching chunks. (Default: 536871424)                                 |
| -MaxMemTableSize<br/>--max-mem-table-size=VALUE<br/>                    | MAX_MEM_TABLE_SIZE                             | MaxMemTableSize          | Adjusts the maximum size of a mem table. (Default: 1000000)                                                    |
| -HashCollisionReadLimit<br/>--hash-collision-read-limit=VALUE<br/>      | HASH_COLLISION_READ_LIMIT                      | HashCollisionReadLimit   | The number of events to read per candidate in the case of a hash collision (Default: 100)                      |
| -Db<br/>--db=VALUE<br/>                                                 | DB                                             | Db                       | The path the db should be loaded/saved to. (Default: [See default directories](default-directories.md))        |
| -Index<br/>--index=VALUE<br/>                                           | INDEX                                          | Index                    | The path the index should be loaded/saved to.                                                                  |
| -MemDb<br/>--mem-db=VALUE<br/>                                          | MEM_DB                                         | MemDb                    | Keep everything in memory, no directories or files are created. (Default: False)                               |
| -SkipDbVerify<br/>--skip-db-verify=VALUE<br/>                           | SKIP_DB_VERIFY                                 | SkipDbVerify             | Bypasses the checking of file hashes of database during startup (allows for faster startup). (Default: False)  |
| -WriteThrough<br/>--write-through=VALUE<br/>                            | WRITE_THROUGH                                  | WriteThrough             | Enables Write Through when writing to the file system, this bypasses filesystem caches. (Default: False)       |
| -Unbuffered<br/>--unbuffered=VALUE<br/>                                 | UNBUFFERED                                     | Unbuffered               | Enables Unbuffered/DirectIO when writing to the file system, this bypasses filesystem caches. (Default: False) |
| -PrepareTimeoutMs<br/>--prepare-timeout-ms=VALUE<br/>                   | PREPARE_TIMEOUT_MS                             | PrepareTimeoutMs         | Prepare timeout (in milliseconds). (Default: 2000)                                                             |
| -CommitTimeoutMs<br/>--commit-timeout-ms=VALUE<br/>                     | COMMIT_TIMEOUT_MS                              | CommitTimeoutMs          | Commit timeout (in milliseconds). (Default: 2000)                                                              |
| -UnsafeDisableFlushToDisk<br/>--unsafe-disable-flush-to-disk=VALUE<br/> | UNSAFE_DISABLE_FLUSH_TO_DISK                   | UnsafeDisableFlushToDisk | Disable flushing to disk.  (UNSAFE: on power off) (Default: False)                                             |
| -BetterOrdering<br/>--better-ordering=VALUE<br/>                        | BETTER_ORDERING                                | BetterOrdering           | Enable Queue affinity on reads during write process to try to get better ordering. (Default: False)            |
| -UnsafeIgnoreHardDelete<br/>--unsafe-ignore-hard-delete=VALUE<br/>      | UNSAFE_IGNORE_HARD_DELETE                      | UnsafeIgnoreHardDelete   | Disables Hard Deletes (UNSAFE: use to remove hard deletes) (Default: False)                                    |
| -IndexCacheDepth<br/>--index-cache-depth=VALUE<br/>                     | INDEX_CACHE_DEPTH                              | IndexCacheDepth          | Sets the depth to cache for the mid point cache in index. (Default: 16)                                        |
| -AlwaysKeepScavenged<br/>--always-keep-scavenged=VALUE<br/>             | ALWAYS_KEEP_SCAVENGED                          | AlwaysKeepScavenged      | Always keeps the newer chunks from a scavenge operation. (Default: False)                                      |

### Interface Options

| Parameter                                                                                     | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml                                | Description                                                                                                            |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| -IntIp<br/>--int-ip=VALUE<br/>                                                                | INT_IP                                         | IntIp                               | Internal IP Address. (Default: 127.0.0.1)                                                                              |
| -ExtIp<br/>--ext-ip=VALUE<br/>                                                                | EXT_IP                                         | ExtIp                               | External IP Address. (Default: 127.0.0.1)                                                                              |
| -IntHttpPort<br/>--int-http-port=VALUE<br/>                                                   | INT_HTTP_PORT                                  | IntHttpPort                         | Internal HTTP Port. (Default: 2112)                                                                                    |
| -ExtHttpPort<br/>--ext-http-port=VALUE<br/>                                                   | EXT_HTTP_PORT                                  | ExtHttpPort                         | External HTTP Port. (Default: 2113)                                                                                    |
| -IntTcpPort<br/>--int-tcp-port=VALUE<br/>                                                     | INT_TCP_PORT                                   | IntTcpPort                          | Internal TCP Port. (Default: 1112)                                                                                     |
| -IntSecureTcpPort<br/>--int-secure-tcp-port=VALUE<br/>                                        | INT_SECURE_TCP_PORT                            | IntSecureTcpPort                    | Internal Secure TCP Port. (Default: 0)                                                                                 |
| -ExtTcpPort<br/>--ext-tcp-port=VALUE<br/>                                                     | EXT_TCP_PORT                                   | ExtTcpPort                          | External TCP Port. (Default: 1113)                                                                                     |
| -ExtSecureTcpPortAdvertiseAs<br/>--ext-secure-tcp-port-advertise-as=VALUE<br/>                | EXT_SECURE_TCP_PORT_ADVERTISE_AS               | ExtSecureTcpPortAdvertiseAs         | Advertise Secure External Tcp Port As. (Default: 0)                                                                    |
| -ExtSecureTcpPort<br/>--ext-secure-tcp-port=VALUE<br/>                                        | EXT_SECURE_TCP_PORT                            | ExtSecureTcpPort                    | External Secure TCP Port. (Default: 0)                                                                                 |
| -ExtIpAdvertiseAs<br/>--ext-ip-advertise-as=VALUE<br/>                                        | EXT_IP_ADVERTISE_AS                            | ExtIpAdvertiseAs                    | Advertise External Tcp Address As.                                                                                     |
| -ExtTcpPortAdvertiseAs<br/>--ext-tcp-port-advertise-as=VALUE<br/>                             | EXT_TCP_PORT_ADVERTISE_AS                      | ExtTcpPortAdvertiseAs               | Advertise External Tcp Port As. (Default: 0)                                                                           |
| -ExtHttpPortAdvertiseAs<br/>--ext-http-port-advertise-as=VALUE<br/>                           | EXT_HTTP_PORT_ADVERTISE_AS                     | ExtHttpPortAdvertiseAs              | Advertise External Http Port As. (Default: 0)                                                                          |
| -IntIpAdvertiseAs<br/>--int-ip-advertise-as=VALUE<br/>                                        | INT_IP_ADVERTISE_AS                            | IntIpAdvertiseAs                    | Advertise Internal Tcp Address As.                                                                                     |
| -IntSecureTcpPortAdvertiseAs<br/>--int-secure-tcp-port-advertise-as=VALUE<br/>                | INT_SECURE_TCP_PORT_ADVERTISE_AS               | IntSecureTcpPortAdvertiseAs         | Advertise Secure Internal Tcp Port As. (Default: 0)                                                                    |
| -IntTcpPortAdvertiseAs<br/>--int-tcp-port-advertise-as=VALUE<br/>                             | INT_TCP_PORT_ADVERTISE_AS                      | IntTcpPortAdvertiseAs               | Advertise Internal Tcp Port As. (Default: 0)                                                                           |
| -IntHttpPortAdvertiseAs<br/>--int-http-port-advertise-as=VALUE<br/>                           | INT_HTTP_PORT_ADVERTISE_AS                     | IntHttpPortAdvertiseAs              | Advertise Internal Http Port As. (Default: 0)                                                                          |
| -IntTcpHeartbeatTimeout<br/>--int-tcp-heartbeat-timeout=VALUE<br/>                            | INT_TCP_HEARTBEAT_TIMEOUT                      | IntTcpHeartbeatTimeout              | Heartbeat timeout for internal TCP sockets (Default: 700)                                                              |
| -ExtTcpHeartbeatTimeout<br/>--ext-tcp-heartbeat-timeout=VALUE<br/>                            | EXT_TCP_HEARTBEAT_TIMEOUT                      | ExtTcpHeartbeatTimeout              | Heartbeat timeout for external TCP sockets (Default: 1000)                                                             |
| -IntTcpHeartbeatInterval<br/>--int-tcp-heartbeat-interval=VALUE<br/>                          | INT_TCP_HEARTBEAT_INTERVAL                     | IntTcpHeartbeatInterval             | Heartbeat interval for internal TCP sockets (Default: 700)                                                             |
| -ExtTcpHeartbeatInterval<br/>--ext-tcp-heartbeat-interval=VALUE<br/>                          | EXT_TCP_HEARTBEAT_INTERVAL                     | ExtTcpHeartbeatInterval             | Heartbeat interval for external TCP sockets (Default: 2000)                                                            |
| -GossipOnSingleNode<br/>--gossip-on-single-node=VALUE<br/>                                    | GOSSIP_ON_SINGLE_NODE                          | GossipOnSingleNode                  | When enabled tells a single node to run gossip as if it is a cluster (Default: False)                                  |
| -AdminOnExt<br/>--admin-on-ext=VALUE<br/>                                                     | ADMIN_ON_EXT                                   | AdminOnExt                          | Whether or not to run the admin ui on the external HTTP endpoint (Default: True)                                       |
| -StatsOnExt<br/>--stats-on-ext=VALUE<br/>                                                     | STATS_ON_EXT                                   | StatsOnExt                          | Whether or not to accept statistics requests on the external HTTP endpoint, needed if you use admin ui (Default: True) |
| -GossipOnExt<br/>--gossip-on-ext=VALUE<br/>                                                   | GOSSIP_ON_EXT                                  | GossipOnExt                         | Whether or not to accept gossip requests on the external HTTP endpoint (Default: True)                                 |
| -IntHttpPrefixes<br/>--int-http-prefixes=VALUE<br/>                                           | INT_HTTP_PREFIXES                              | IntHttpPrefixes                     | The prefixes that the internal HTTP server should respond to. (Default: n/a)                                           |
| -ExtHttpPrefixes<br/>--ext-http-prefixes=VALUE<br/>                                           | EXT_HTTP_PREFIXES                              | ExtHttpPrefixes                     | The prefixes that the external HTTP server should respond to. (Default: n/a)                                           |
| -EnableTrustedAuth<br/>--enable-trusted-auth=VALUE<br/>                                       | ENABLE_TRUSTED_AUTH                            | EnableTrustedAuth                   | Enables trusted authentication by an intermediary in the HTTP (Default: False)                                         |
| -AddInterfacePrefixes<br/>--add-interface-prefixes=VALUE<br/>                                 | ADD_INTERFACE_PREFIXES                         | AddInterfacePrefixes                | Add interface prefixes (Default: True)                                                                                 |
| -UseInternalSsl<br/>--use-internal-ssl=VALUE<br/>                                             | USE_INTERNAL_SSL                               | UseInternalSsl                      | Whether to use secure internal communication. (Default: False)                                                         |
| -DisableInsecureTCP<br/>--disable-insecure-tcp=VALUE<br/>                                     | DISABLE_INSECURE_TCP                           | DisableInsecureTCP                  | Whether to disable insecure TCP communication (Default: False)                                                         |
| -SslTargetHost<br/>--ssl-target-host=VALUE<br/>                                               | SSL_TARGET_HOST                                | SslTargetHost                       | Target host of server's SSL certificate. (Default: n/a)                                                                |
| -SslValidateServer<br/>--ssl-validate-server=VALUE<br/>                                       | SSL_VALIDATE_SERVER                            | SslValidateServer                   | Whether to validate that server's certificate is trusted. (Default: True)                                              |
| -ConnectionPendingSendBytesThreshold<br/>--connection-pending-send-bytes-threshold=VALUE<br/> | CONNECTION_PENDING_SEND_BYTES_THRESHOLD        | ConnectionPendingSendBytesThreshold | The maximum number of pending send bytes allowed before a connection is closed. (Default: 10485760)                    |

### Projections Options

| Parameter                                              | Environment _(all prefixed with EVENTSTORE\_)_ | Yaml              | Description                                                                                                                                      |
| ------------------------------------------------------ | ---------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| -RunProjections<br/>--run-projections=VALUE<br/>       | RUN_PROJECTIONS                                | RunProjections    | Enables the running of projections. System runs built-in projections, All runs user projections. (Default: None) Possible Values:None,System,All |
| -ProjectionThreads<br/>--projection-threads=VALUE<br/> | PROJECTION_THREADS                             | ProjectionThreads | The number of threads to use for projections. (Default: 3)                                                                                       |
