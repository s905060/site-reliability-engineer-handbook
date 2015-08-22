# How-To Compile Linux Kernel from Source

```
# cd /usr/src/
# wget https://www.kernel.org/pub/linux/kernel/v3.x/linux-3.9.3.tar.xz
#tar -xvJf linux-3.9.3.tar.xz
# cd linux-3.9.3
# make menuconfig
# make
# make modules
# make modules_install
# make install
# reboot
$ uname -r
3.9.3
```
The make install command will create the following files in the /boot directory.

* vmlinuz-3.9.3 – The actual kernel
* System.map-3.9.3 – The symbols exported by the kernel
* initrd.img-3.9.3 – initrd image is temporary root file system used during boot process
* config-3.9.3 – The kernel configuration file

The command “make install” will also update the grub.cfg by default. So we don’t need to manually edit the grub.cfg file.
