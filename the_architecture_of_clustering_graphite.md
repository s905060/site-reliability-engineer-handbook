# The architecture of clustering Graphite

###Clarifying the Components

Graphite isn't directly comparable to say, Zenoss. It's not a monolithic, boxed-up, poll-based SNMP system. It scales and bottlenecks a bit differently than what you might be used to. It's the fundamental operations that need to be understood in order to break out the components properly, and I think this is where the 'installation guides' I've mentioned skimp out.

So while you can find plenty of those (in case you are looking for basic installation), I'll go over what the components are and how they interact.

###Graphite

In whole is basically a collection of a few components that allow for the aggregation, storage and querying of metrics from many hosts. Graphite itself does not provide agents for collecting data on monitored hosts, default dashboards or alerting functionality. Consider it a "metrics core" on which a full monitoring / alerting system can be built by adding additional layers.

The core components:

* Carbon
    * An event-driven, Python-based daemon that listens on a TCP port, expecting a stream of time-series data.
    * Time-series data in concept: someMetric:someValue:timeStamp
    * Carbon expects time-series data in a particular format (of two primary types - details later). Third party tools that support * Graphite are used to feed properly formatted data, such as Collectd or StatsD.
    * Metrics can be anything from OS memory usage to event counts fired off from an application (e.g. number of times a function was called).
    * After Carbon receives metrics, it periodically flushes them to a storage database.

* Whisper
    * A lightweight, flat-file database format for storing time-series data.
    * It does not run as a stand-alone service or bind to a port. * Carbon natively supports writing to disk in "Whisper format".
    * Each unique metric type is stored in a fixed-size file. If you fed in the metrics memory free and memory used for both Host A and Host B, the following database files would be created:
```
$WHISPER_DIR/carbon/whisper/HostA/memory-free.wsp 
$WHISPER_DIR/carbon/whisper/HostA/memory-used.wsp 
$WHISPER_DIR/carbon/whisper/HostB/memory-free.wsp 
$WHISPER_DIR/carbon/whisper/HostB/memory-used.wsp
```
    * The size of database files is determined by the number of data points stored - this is configurable (details later).