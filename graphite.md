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