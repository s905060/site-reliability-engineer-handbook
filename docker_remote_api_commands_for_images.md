# Docker Remote API Commands for Images

In the last post of this series, we discussed Docker Remote API and explored the commands specific to containers. In this post, let’s discuss commands specific to images.

### Create an Image

Images can be created in one of the following ways:

* By performing a registry pull
* By importing the image
```
POST /images/create
```
A sample request is shown in this screenshot.