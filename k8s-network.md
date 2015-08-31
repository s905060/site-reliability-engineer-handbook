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

How to achieve this:
1. Google Compute Engine (GCE)
2. L2 networks and linux bridging
Four ways to connect a docker container to a local network
3. Flannel
Flannel is a very simple overlay network that satisfies the Kubernetes requirements.
4. OpenVSwitch
OpenVSwitch is a somewhat more mature but also complicated way to build an overlay network. Kubernetes OpenVSwitch GRE/VxLAN networking
5. Weave
Weave is yet another way to build an overlay network, primarily aiming at Docker integration.
6. Calico
Calico uses BGP to enable real container IPs.

### Kubernetes OpenVSwitch GRE/VxLAN networking
This document describes how OpenVSwitch is used to setup networking between pods across nodes. The tunnel type could be GRE or VxLAN. VxLAN is preferable when large scale isolation needs to be performed within the network.

