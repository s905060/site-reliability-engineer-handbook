# Graphite

In this article, I'll provide a guide to help through all of the steps involved in setting up a monitoring system using a Graphite stack.

###What We Will Cover

We will cover the following topics to set up our Graphite monitoring system:

1. Introduction to Carbon & Whisper
2. Whisper Storage Schemas & Aggregations
3. Graphite Webapp

###Prerequisites

First and foremost we need hardware on which to run the Graphite stack. For simplicity, I will be using Amazon Web Services EC2 hosts. However, feel free to use any type of computer that you might have laying around in your office or at home.

Specifications:

* Operating System: Red Hat Enterprise Linux (RHEL) 6.5
* Instance Type: m3.xlarge
* Elastic Block Store (EBS) Volume: 250 GB
* Python Version: 2.6.6

###Introduction to Carbon & Whisper

Graphite is composed of multiple back-end and front-end components. The back-end components are used to store numeric time-series data. The front-end components are used to retrieve the metric data and optionally render graphs. In this article, I'll focus first on the back-end components: Carbon and Whisper.

![](2fig1.png)

Metrics can be published to a load balancer or directly to a Carbon process. The Carbon process interacts with the Whisper database library to store the time-series data to the filesystem.

**Install Carbon**

Carbon refers to a series of daemons that make up the storage backend of a Graphite installation. The daemons listen for time-series data using an event-driven networking engine called Twisted. The Twisted framework permits Carbon daemons to handle a large number of clients and a large amount of traffic with a low amount of overhead.

To install Carbon, run the following commands (assuming RHEL operating system):

```
# sudo yum groupinstall "Development Tools"
# sudo yum install python-devel
# sudo yum install git
# sudo easy_install pip
# sudo pip install twisted
# cd /tmp
# git clone https://github.com/graphite-project/carbon.git
# cd /tmp/carbon
# sudo python setup.py install
```

The /opt/graphite directory should now have the carbon libraries and configuration files:

```
# ls -l /opt/graphite
drwxr-xr-x. 2 root root 4096 May 18 23:56 bin
drwxr-xr-x. 2 root root 4096 May 18 23:56 conf
drwxr-xr-x. 4 root root 4096 May 18 23:56 lib
drwxr-xr-x. 6 root root 4096 May 18 23:56 storage
```

Inside the bin folder, you’ll find the three different types of Carbon daemons.

* **Cache**: accepts metrics over various protocols and writes them to disk as efficiently as possible; caches metric values in RAM as they are received, and flushes them to disk on a specified interval using the underlying Whisper library.
* **Relay**: serves two distinct purposes: replication and sharding of incoming metrics.
* **Aggregator**: runs in front of a cache to buffer metrics over time before reporting them into Whisper.

###Install Whisper

Whisper is a database library for storing time-series data that is then retrieved and manipulated by applications using the create, update, and fetch operations.

To install Whisper, run the following commands:

```
# cd /tmp
# git clone https://github.com/graphite-project/whisper.git
# cd /tmp/whisper
# sudo python setup.py install
```

The Whisper scripts should now be in place:

```
# ls -l /usr/bin/whisper*
-rwxr-xr-x. 1 root root 1711 May 19 00:00 /usr/bin/whisper-create.py
-rwxr-xr-x. 1 root root 2902 May 19 00:00 /usr/bin/whisper-dump.py
-rwxr-xr-x. 1 root root 1779 May 19 00:00 /usr/bin/whisper-fetch.py
-rwxr-xr-x. 1 root root 1121 May 19 00:00 /usr/bin/whisper-info.py
-rwxr-xr-x. 1 root root  674 May 19 00:00 /usr/bin/whisper-merge.py
-rwxr-xr-x. 1 root root 5982 May 19 00:00 /usr/bin/whisper-resize.py
-rwxr-xr-x. 1 root root 1060 May 19 00:00 /usr/bin/whisper-set-aggregation-method.py
-rwxr-xr-x. 1 root root  969 May 19 00:00 /usr/bin/whisper-update.py
```

###Start a Carbon Cache Process

The Carbon installation comes with sensible defaults for port numbers and many other configuration parameters. Copy the existing example configuration files:
```
# cd /opt/graphite/conf
# cp aggregation-rules.conf.example aggregation-rules.conf
# cp blacklist.conf.example blacklist.conf
# cp carbon.conf.example carbon.conf
# cp carbon.amqp.conf.example carbon.amqp.conf
# cp relay-rules.conf.example relay-rules.conf
# cp rewrite-rules.conf.example rewrite-rules.conf
# cp storage-schemas.conf.example storage-schemas.conf
# cp storage-aggregation.conf.example storage-aggregation.conf
# cp whitelist.conf.example whitelist.conf
# vi carbon.conf
```

Under the cache section, the line receiver port has a default value and it is used to accept incoming metrics through the plaintext protocol (see below):
```
[cache]
LINE_RECEIVER_INTERFACE = 0.0.0.0
LINE_RECEIVER_PORT = 2003
```

Start a carbon-cache process by running the following command:

```
# cd /opt/graphite/bin
# ./carbon-cache.py start
Starting carbon-cache (instance a)
```

The process should now be listening on port 2003:
```
# ps -efla | grep carbon-cache
1 S root      2674     1  0  80   0 - 75916 ep_pol 00:18 ?        00:00:03 /usr/bin/python ./carbon-cache.py start

# netstat -nap | grep 2003
tcp        0      0 0.0.0.0:2003                0.0.0.0:*                   LISTEN      2674/python 
```

###Publish Metrics

A metric is any measurable quantity that can vary over time, for example:

* number of requests per second
* request processing time
* CPU usage

A datapoint is a tuple containing:

* a metric name
* a measured value
* at a specific point in time (usually a timestamp)

Client applications publish metrics by sending data points to a Carbon process. The application establishes a TCP connection on the Carbon process' port and sends data points in a simple plaintext format. In our example, the port is 2003. The TCP connection may remain open and reused as many times as necessary. The Carbon process listens for incoming data but does not send any response back to the client.

The datapoint format is defined as:

* a single line of text per data point
* a dotted metric name at position 0
* a value at position 1
* a Unix Epoch timestamp at position 2
* spaces for the position separators

For example, here are some valid datapoints:

* The number of metrics received by the carbon-cache process every minute
    * carbon.agents.graphite-tutorial.metricsReceived 28198 1400509108
* The number of metrics created by the carbon-cache process every minute
    * carbon.agents.graphite-tutorial.creates 8 1400509110
* The p95 response times for a sample server endpoint over a minute
    * PRODUCTION.host.graphite-tutorial.responseTime.p95 0.10 1400509112

Client applications have multiple ways to publish metrics:

* using the plaintext protocol with tools such as the netcat (nc) command
* using the pickle protocol
* using the Advanced Message Queueing Protocol (AMQP)
* using libraries such as the Dropwizard Metrics library

For simplicity, in this tutorial I'll be using the plaintext protocol through the netcat command. To publish the example datapoints listed above, run the following commands:

```
sudo yum install nc
echo "carbon.agents.graphite-tutorial.metricsReceived 28198 `date +%s`" | nc localhost 2003
echo "carbon.agents.graphite-tutorial.creates 8 `date +%s`" | nc localhost 2003
echo "PRODUCTION.host.graphite-tutorial.responseTime.p95 0.10 `date +%s`" | nc localhost 2003
```

The carbon-cache log files will contain information about the new metrics received and where the information was stored:
```
# tail -f /opt/graphite/storage/log/carbon-cache/carbon-cache-a/creates.log
19/05/2014 10:42:44 :: creating database file /opt/graphite/storage/whisper/carbon/agents/graphite-tutorial/metricsReceived.wsp (archive=[(60, 129600)] xff=0.5 agg=average)
19/05/2014 10:42:53 :: creating database file /opt/graphite/storage/whisper/carbon/agents/graphite-tutorial/creates.wsp (archive=[(60, 129600)] xff=0.5 agg=average)
19/05/2014 10:42:57 :: creating database file /opt/graphite/storage/whisper/PRODUCTION/host/graphite-tutorial/responseTime/p95.wsp (archive=[(60, 1440)] xff=0.5 agg=average)
```

Carbon interacts with Whisper to store the time-series data to the filesystem. Navigate the filesystem to make sure the data files have been created:
```
# ls -l /opt/graphite/storage/whisper/carbon/agents/graphite-tutorial/
total 3040
-rw-r--r--. 1 root root 1555228 May 19 10:42 creates.wsp
-rw-r--r--. 1 root root 1555228 May 19 10:42 metricsReceived.wsp
# ls -l /opt/graphite/storage/whisper/PRODUCTION/host/graphite-tutorial/responseTime/
total 20
-rw-r--r--. 1 root root 17308 May 19 10:42 p95.wsp
```