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

###Method Two
###dd an empty disk

`sudo dd if = / dev / Zero of = / home / VM1 . img bs = 1G count = 8`

###Formatting a disk

`sudo dd if = / dev / Zero of = / home / VM1 . img bs = 1G count = 8`

###Mount disk
```
sudo mkdir / mnt / VM1
sudo Mount - O loop / home / VM1 . img / mnt / VM1
```

###Use Domain0 file system creates the file system DomainU

```
#! / Bin / bash 
cat >  / tmp / exclude . List << EOF
 / proc
 / tmp /
/ Lost + found
 / sys
 / mnt
 / Media
 / dev
 / tmp
 / home
 / var / cache / apt
 / var / cache / apt - Xapian - index
 / var / lib / apt
EOF

# Attention back vmdisk variables / 
vmdisk = '/ mnt /' 
rsync - arv - Progress - exclude - from = / tmp / exclude . List / $ vmdisk
mkdir - P $ { vmdisk } / home $ { vmdisk } / mnt $ { vmdisk } / tmp $ { vmdisk } / dev $ { vmdisk } / proc $ { vmdisk } / sys
```

###Uninstall virtual machine disk

`sudo umount   / mnt / VM1`

###Creating a virtual machine configuration file
editor vim /etc/xen/vm1.conf follows
```
name        =  'VM1' 
VCPUs       =   1 
Memory      =  '2048' 
Disk        =  [  'TAP2: tapdisk: aio: /home/vm1/vm1.img,xvda,w' ] 
vif         =  [  ''  ] 
on_reboot   =  'restart' 
on_crash    =  'restart' 
kernel      =  "/ home / VM1 / vmlinuz" 
ramdisk     =  "/home/vm1/initrd.img" 
Extra       =  "KS = HTTP: //www.opstool.com/files/man/vm-ks.cfg"
```

###Start the virtual machine

`XM sudo create - c / etc / xen / VM1 . conf`