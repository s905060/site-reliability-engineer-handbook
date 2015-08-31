# K8S-Network

![](Screen Shot 2015-08-30 at 9.57.44 PM.png)

1. Highly-coupled container-to-container communications.
2. Pop-to-Pod communications.
3. Pod-to-Service communications.
4. External-to-Service communications.

Kubernetes model
Kubernetes imposes some fundamental requirements on any networking implementation:

* All containers can communicate with all other containers without NAT.

* All nodes can communicate with all containers without NAT.

* The IP that a container sees itself as is the same IP that others see it as.