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

