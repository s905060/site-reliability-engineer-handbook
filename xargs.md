# Xargs

Copy all images to external hard-drive

```
# ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory
``

Search all jpg images in the system and archive it.

```
# find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz
```

Download all the URLs mentioned in the url-list.txt file

```
# cat url-list.txt | xargs wget –c
```