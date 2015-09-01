# Automation is the Word Using DockerFile!

In our last Docker tutorial series post, we shared the 15 commands that got us onboard with Docker. This set of Docker commands are steps to manually create images. That is to basically help create images, as well as commit, search, pull and push images.

But why opt for the long tedious way of creating images when it can all be automated. So, let’s automate!

Docker offers us the automation solution as DockerFile. In this post, we will discuss what a Dockerfile is, what it is capable of doing, and some basic DockerFile syntax.

### Commands for Ease of Automation

DockerFile is a script that houses instructions needed to create an image. The image can then be created based on the instructions in the DockerFile using the #Docker build command. It simplifies deployment of an image by easing the entire image and container creation process.

DockerFiles support commands in the following syntax:
```
INSTRUCTION argument
```
Instructions are not case sensitive. However, they are capitalized for a naming convention.

All DockerFiles must begin with the “FROM” command. The “FROM” command indicates the base image from which the new image will be created and from which all subsequent instructions will follow. The “FROM” command can be used any number of times, indicating the creation of multiple images. The syntax is as follows:
```
FROM <image name>
```
```
FROM ubuntu
```
tells us that the new image will be created from the base Ubuntu image.