# Simple Docker

###History

Old way

```
server : application 1 1
```

Current way:
```
OS    OS    OS     <== consumes resources (RAM, HD, CPU) before even an app installed /licensing  | better security
VM1 VM2 VM2..   x 10
<— Hypervisor —>
Physical Machine x 1
```

###What is a container?
```
app               app               app
container      container      container  
<— OS  —>
<— VM  —>
```

The single ‘user space’ is now a within a container which allows for multiple user spaces via containers. Each container has its user space and app.

Question! Why not just install all apps within a ‘user space’ or container? We cannot share resources between applications. This filesystem, pid etc.

Containers are essentially runtime environment which consume low resources.

Question ! Can a container run on multiple operating systems?

No, containers use the OS kernel.

###How do containers work?

* Each container will have its own root file system
* Isolated process trees
* Isolated network stack
* Isolated users

###How do you achieve this?

* kernal namespaces
I.e: pid mnt net user

* cgroups Helps us group together resources and apply limits (flex resources)

Map a cgroup to a container and set limits to memory, cpu. block IO has access to cgroup = container

* Capabilities Fine grain control on privileges