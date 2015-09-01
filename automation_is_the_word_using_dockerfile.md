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

Following the “FROM” command, DockerFile offers several other commands that ease automation. The order of the commands in the simple text file, or DockerFile, is the order in which it is executed.

Let’s walk through some interesting DockerFile commands.

1. MAINTAINER: Set an author field for the image using this instruction. The simple and obvious syntax is:
```
MAINTAINER <author name>
```
2. RUN: Execute a command in a shell or exec form. The RUN instruction adds a new layer on top of the newly created image. The committed results are then used for the next instruction in the DockerFile.
```
Syntax: RUN <command>
```
3. ADD: Copy files from one location to another using the ADD instruction. It takes two arguments <source> and <destination>. The destination is a path in the container. The source can be either a URL or a file in the context of the launch config.
```
<em><strong>Syntax:</strong></em> ADD &lt;src&gt; &lt;destination&gt;
```
4. CMD: Defaults for an executing container are provided using the CMD command. DockerFile allows usage of the CMD instruction only once. Multiple usage of CMD nullifies all previous CMD instructions. CMD comes in three flavours:
```
Syntax:CMD ["executable","param1","param2"]
```
```
CMD ["param1","param2"]
```
```
CMD command param1 param2
```