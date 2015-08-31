# The Next 15 Docker Commands

In an earlier post for the Docker Tutorial Series, we discussed the first 15 Docker commands. We shared some hands-on experience in how they are used and what they do.

In this post, we will talk about another 15 Docker commands, leading us to a more practical experience using Docker.

Here they are:

### daemon:

Docker daemon is the persistent background process that helps manage containers. In general, daemon is a long running process servicing requests for services.

-d flag is used to run the daemon

### build:

As discussed before, Docker images can be built using dockerfiles. A simple command for the build is as follows:
```
docker build [options] PATH | URL
```
There are also some interesting options that Docker provides, such as:

--rm=true; all intermediate containers are removed after a successful build

--no-cache=false; avoids using cache during build

The following screenshot depicts the use of the Docker build command.

### attach:

Docker allows interaction with running containers using the attach command. The command also allows viewing of daemonized processes. Detaching from the container can be done in two ways:

Ctrl+c - for a quiet exit

Ctrl-\ - to detach with a stack trace

The syntax for attach is
```
docker attach container
```
This screenshot shows a simple execution of the attach command.

### diff:

Docker provides a very powerful command diff which lists the changes in the files and directories. The changes include addition, deletion and those represented by the A, D and C flags, respectively. This command improves debugging processes and allows faster sharing of environments.

The syntax is
```
docker diff container
```
Here’s a screenshot of the diff command executed.

### events:

Real-time details of events can be collected from the server by specifying the duration for which the real-time data needs to be collected.

### import:

Docker allows imports from remote locations and a local file or directory. Import from remote locations is done using http, and imports from local files or directories is accomplished using the “-” parameter.

The syntax for import from a remote location is
```
docker import http://example.com/example.tar
```
The following image illustrates import from a local file.

### export:

Similar to import, the export command is used to transfer the contents of the filesystem as a tar file. The following image depicts a simple execution of the same.

cp:

This command copies files from the containers filesystems to the specified path. The syntax for the cp command is
```
docker cp container:path hostpath.
```
This image depicts the working of the cp command.

