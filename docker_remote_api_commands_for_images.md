# Docker Remote API Commands for Images

In the last post of this series, we discussed Docker Remote API and explored the commands specific to containers. In this post, let’s discuss commands specific to images.

### Create an Image

Images can be created in one of the following ways:

* By performing a registry pull
* By importing the image
```
POST /images/create
```

### Create an Image from a Container

To create an image from the container’s commits, use:
```
POST /commit
```

### List of Images

To obtain the list of images, use:
```
GET /images/json
```