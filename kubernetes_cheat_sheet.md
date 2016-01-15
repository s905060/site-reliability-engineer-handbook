# Kubernetes Cheat Sheet

###Physical Layout

![](k8s-cheatsheet-physical-layout.png)

Query the server/client version used:
`kubectl version`

Get cluster info:
`kubectl cluster-info`

Get configuration info:
`kubectl config view`

Watch nodes continuously:
`kubectl get nodes -w`

Get info about 'node123':
`kubectl describe node123`

From a physical perspective, a Kubernetes cluster consists of:

1. A master (with several independent sub-components, details below) that coordinates the work.
2. A distributed key-value store, currently etcd, for maintaining the resource state in a persistent and reliable manner, throughout the cluster.
3. A number of nodes that carry out the work.
4. A command line tool called `kubectl` allowing to query and manipulate the cluster state; this is a fancy way of saying: running containers, creating services and administrating the cluster (logging, monitoring, debugging).