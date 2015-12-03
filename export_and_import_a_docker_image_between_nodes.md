# EXPORT AND IMPORT A DOCKER IMAGE BETWEEN NODES

```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f4b0d7285fec        ubuntu:14.04        /bin/bash           38 minutes ago      Exit 0                                  hungry_thompson
8ae64c0faa34        ubuntu:14.04        /bin/bash           41 minutes ago      Exit 0                                  jovial_hawking
3a09b2588478        ubuntu:14.04        /bin/bash           45 minutes ago      Exit 0                                  kickass_lovelace
```

```
$ docker commit 3a09b2588478 mynewimage
4d2eab1c0b9a13c83abd72b38e5d4b4315de3c9967165f78a7b817ca99bf191e
```

```
$ docker save mynewimage > /tmp/mynewimage.tar

```

```
$ docker load < /tmp/mynewimage.tar
```

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
mynewimage          latest              4d2eab1c0b9a        5 minutes ago       278.1 MB
ubuntu              14.04               ad892dd21d60        11 days ago         275.5 MB
<none>              <none>              6b0a59aa7c48        11 days ago         169.4 MB
<none>              <none>              6cfa4d1f33fb        7 weeks ago         0 B
```