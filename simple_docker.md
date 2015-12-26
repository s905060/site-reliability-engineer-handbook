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

###What is Docker?
```
Docker 
    |
[OS [Kernel[ namespaces| cgroups | capabilities ]]]
```

Docker is an implementation of a container. It provides a standard runtime for apps.

###What is the difference between Docker and LXC?

Docker previously used `LXC` as the execution driver to talk to the kernel.

`LXC` version needs to be compatible with Docker and because it is not developed by docker, now, docker uses the `libcontainer` as the default execution driver.

###What is the docker client and daemon?

The docker client sends commands to the docker daemon. The docker daemon is responsible for setting up containers which includes setting up cgroups, capabilities etc.

###Installation

On centos 6.5 do:
```
sudo yum install docker-io

# Check info
sudo docker info
sudo docker version
```

My docker instance failed to start with the following error:

```
/usr/bin/docker: relocation error: /usr/bin/docker: symbol dm_task_get_info_with_deferred_remove, version Base not defined in file libdevmapper.so.1.02 with link time reference
```

I just needed to update the device-mapper-event-libs package.

Reference:

http://stackoverflow.com/questions/27216473/docker-1-3-fails-to-start-on-rhel6-5