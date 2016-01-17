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