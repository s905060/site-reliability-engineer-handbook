# The 15 Commands

In part one of our #Docker Tutorial Series, we learned about the basics of #Docker. We examined how it works and how it’s installed. In this post, let’s now learn the 15 Docker commands and get some hands-on experience in how they are used and what they do.

First, let’s check if Docker is properly installed by running the following command:
```
docker info 
```
If this command is not found, then it indicates #Docker has not been properly installed. A proper install would look similar to the following image:

At this point, there are no images or containers. So, let’s create one by pulling a pre-built image using the command:
```
sudo docker pull busybox
```

At this point, there are no images or containers. So, let’s create one by pulling a pre-built image using the command:
```
sudo docker pull busybox
```

BusyBox is a minimal Linux system that provides major functionalities, except for some features and options of its GNU counterparts.

The next step is to run the traditional, mundane, yet significant, “hello world.” For a change, let’s call it “Hello Docker.”
```
docker run busybox /bin/echo Hello Docker
```

Now, let’s run hello docker as a long running process:
```
sample_job=$(docker run -d busybox /bin/sh -c “while true; do echo Docker; sleep 1; done”)
```

The command sample_job is the name of the long running job that prints Docker with a delay of one second. The output of the job can be viewed using the Docker logs. If a name is not assigned, the job is assigned a long id, and it might be difficult to use it for further commands such as for the Docker logs.

Run the Docker logs command to view the current state of the job:
```
docker logs $sample_job
```

All Docker commands are readily available by running the help command:
```
docker help
```
The sample_job is the new container, and it can be stopped using the command:
```
docker stop $sample_job
```
Restart the container using the command:
```
docker restart $sample_job
```
To remove a container fully, it needs to be stopped first and then removed. Use this:
```
docker stop $sample_job
```
```
docker rm $sample_job
```
To save the container state as an image, use the command:
```
docker commit $sample_job job1
```
Please note that the image name takes characters [a-z] and numbers [0-9].

Now, you are ready to find the list of all images using the command:
```
docker images
```
In the our previous #Docker Tutorial post, we discovered that images are stored in the #Docker registry. Images in the registry can be searched by running the following command:
```
docker search <image-name>
```
The history of the images can be found by executing this command:
```
docker history <image_name>
```
Finally, to push an image to the registry, use the command:

docker push NAME
You must know that it is very important that the repository is not the root repository, and it should be in the <user>/<repo_name> format.

These are some very basic Docker commands. In our next post for the Docker Tutorial Series, we’ll discuss how to use #Docker to run a Python Web Application as well as some advanced Docker commands.