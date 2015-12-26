# Xen virtual machines installed on Ubuntu

###method one
###dd an empty disk

`sudo dd if = / dev / Zero of = / home / VM1 . img bs = 1G count = 8`

###Download Xen VM generic configuration file

`sudo wget HTTP : //mirrors.aliyun.com/ubuntu/dists/precise/main/installer-amd64/current/images/netboot/xen/xm-debian.cfg \ 
- O / etc / xen / VM1 . conf`

To download the configuration file to be modified

```
Memory =  256 
name =  "VM1" 
Disk    =  [  'TAP2: tapdisk: aio: /home/vm1/vm1.img,xvda1,w' ]
```

###Run the install command
```
XM sudo create - f / etc / xen / VM1 . conf   - c install = true \
install - kernel = "http://mirrors.aliyun.com/ubuntu/dists/precise/main/installer-amd64/current/images/netboot/xen/vmlinuz" \
install - ramdisk = "http://mirrors.aliyun.com/ubuntu/dists/precise/main/installer-amd64/current/images/netboot/xen/initrd.gz" \
install - Mirror = "http://mirrors.aliyun.com/ubuntu"
```