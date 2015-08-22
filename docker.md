# Docker

### Containers:
###### Lifecycle
````
docker create creates a container but does not start it.
docker run creates and starts a container in one operation.
docker stop stops it.
docker start will start it again.
docker restart restarts a container.
docker rm deletes a container.
docker kill sends a SIGKILL to a container.
docker attach will connect to a running container.
docker wait blocks until container stops.
````
###### Info
```
docker ps shows running containers.
docker logs gets logs from container.
docker inspect looks at all the info on a container (including IP address).
docker events gets events from container.
docker port shows public facing port of container.
docker top shows running processes in container.
docker stats shows containers' resource usage statistics.
docker diff shows changed files in the container's FS.
docker ps -a shows running and stopped containers.
```
###### Import / Export
```
There doesn't seem to be a way to use docker directly to import files into a container's filesystem. The closest thing is to mount a host file or directory as a data volume and copy it from inside the container.
docker cp copies files or folders out of a container's 
filesystem.
docker export turns container filesystem into tarball archive stream to STDOUT.
```
###### Executing Commands
```
docker exec to execute a command in container.
```
To enter a running container, attach a new shell process to a running container called foo, use:
```
docker exec -it foo /bin/bash.
```

### Images:
###### Lifecycle
```
docker images shows all images.
docker import creates an image from a tarball.
docker build creates image from Dockerfile.
docker commit creates image from a container.
docker rmi removes an image.
docker insert inserts a file from URL into image. (kind of odd, you'd think images would be immutable after create)
docker load loads an image from a tar archive as STDIN, including images and tags (as of 0.7).
docker save saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions (as of 0.7).
```
###### Info
```
docker history shows history of image.
docker tag tags an image to a name (local or registry).
Docker image ids are sensitive information and should not be exposed to the outside world. Treat them like passwords.
```
###### Registry & Repository:
```
docker login to login to a registry.
docker search searches registry for image.
docker pull pulls an image from registry to local machine.
docker push pushes an image to the registry from local machine.
```

###### Dockerfile:
```
.dockerignore
FROM
MAINTAINER
RUN
CMD
EXPOSE
ENV
ADD
COPY
ENTRYPOINT
VOLUME
USER
WORKDIR
ONBUILD
```