### Learn the Kubernetes Key Concepts in 10 Minutes

In this post I will provide a brief explanation of the key concepts of Kubernetes. I will avoid using lengthy definitions, these are already available in the Kubernetes documentations. Rather, I will be using a few diagrams (some animated) and examples to explain these concepts. I found a few of the concepts difficult to fully grasp without a diagram (Service for example). Where appropriate I will also provide links to the Kubernetes documentations if you want to deep dive.

Let’s start the clock.

###What is Kubernetes?

Kubernetes (k8s) is an open source platform for automating container operations such as deployment, scheduling and scalability across a cluster of nodes.  If you have ever used Docker container technology to deploy your containers, then think of Docker as a low level component used internally by Kubernetes to deploy containers. Not just Docker, but Kubernetes also supports Rocket, another container technology.

Kubernetes orchestrates your containers so together they are performing a Symphony. This could be anything from, but not limited to, the following:

* automate the deployment and replication of containers,
* scale in or out containers on the fly,
* organise containers in groups and provide load balancing between them,
* easily roll out new versions of application containers,
* provide container resilience, if a container dies it gets replaced, etc..

In fact, with Kubernetes you can deploy a full cluster of multi-tiered containers (frontend, backend, etc…) with a single configuration file and a single command:

```
$ kubectl create -f single-config-file.yaml
```

kubectl is a command-line program for interacting with the Kubernetes API. Now let’s introduce some of the key concepts.