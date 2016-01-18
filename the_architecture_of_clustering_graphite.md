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

* Graphite Web
    * A Django web UI that can query Carbon daemons and read Whisper data to return complete metrics data, such as all memory used values logged for Host A over the last 6 hours.
    * Graphite Web can be used directly for composing basic graphs.
    * Graphite Web provides the REST API that can be queried by third-party tools (such as Grafana) to create complete dashboards.
    * The API can return either raw text data or a rendered graph (.png format).

###A Standalone Graphite Setup

So, I think the best favor you can do for yourself is starting with the most basic, necessary components and configuration. The 'install guides' (I've snarkily continued placing in quotes) do the typical thing: include long lists of software packages to install without descriptions of why (Which are actual requirements? Are some just optional and you think they're cool?), followed by arbitrary configurations without explanation (Are these your personal optimizations? Are these configurations necessary to function? Are they unnecessarily elaborate?).

In any case, I will make the following installation recommendations / notes:

* Use PIP to install Carbon / Whisper / Graphite Web if you want it to go down easily.
* Manually install Twisted version 13.1, first: `pip install twisted==13.1`. For some reason I didn't care to sort out, newer versions prevented Graphite daemons from starting properly.

Function first, optimization later.

I'm going to strip out all of the configuration lines that can be left as defaults so that we can focus on the mechanics of the Graphite components.

###Carbon config

Carbon actually has multiple daemons that can be used for different scenarios, and each has their own section in the Carbon config. Carbon-Cache specifically is the core daemon that represents what the Carbon component of Graphite does, so it's the only section that really matters right now:
```
[cache]
LINE_RECEIVER_INTERFACE = 0.0.0.0
LINE_RECEIVER_PORT = 2003
PICKLE_RECEIVER_INTERFACE = 0.0.0.0
PICKLE_RECEIVER_PORT = 2004
CACHE_QUERY_INTERFACE = 0.0.0.0
CACHE_QUERY_PORT = 7002
```

If you recall the mention that Carbon accepts metrics in two different formats, that's what's going on here: