# Anatomy of the initrd and vmlinuz

## vmlinuz:

vmlinuz is the name of the Linux kernel executable. vmlinuz is a compressed Linux kernel, and it is capable of loading the operating system into memory so that the computer becomes usable and application programs can be run.
vmlinuz = Virtual Memory LINUx gZip = Compressed  Linux kernel Executable
vmlinux = Virtual Memory LINUX = Non-compressed  Linux Kernel Executable
At the head of this kernel image (vmlinuz) is a routine that does some minimal amount of hardware setup and then decompresses the kernel contained within the kernel image and places it into high memory. If an initial RAM disk image (initrd) is present, this routine moves it into memory (or we can say extract the compressed ramdisk image in to the real memory) and notes it for later use. The routine then calls the kernel and the kernel boot begins.
 
On Linux systems, vmlinux is a statically linked executable file that contains the Linux kernel in one of the object file formats supported by Linux, which includes ELF, COFF and a.out. The vmlinux file might be required for kernel debugging, symbol table generation or other operations, but must be made bootable before being used as an operating system kernel by adding a multiboot header, bootsector and setup routines.

## initrd:

The initial RAM disk (initrd) is an initial root file system that is mounted prior to when the real rootfile system is available. The initrd is bound to the kernel and loaded as part of the kernel boot procedure. The kernel then mounts this initrd as part of the two-stage boot process to load the modules to make the real file systems available and get at the real root file system.
 
The initrd contains a minimal set of directories and executables to achieve this, such as the insmod tool to install kernel modules into the kernel.
 
Many Linux distributions ship a single, generic Linux kernel image – one that the distribution's developers create specifically to boot on a wide variety of hardware. The device drivers for this generic kernel image are included as loadable kernel modules because statically compiling many drivers into one kernel causes the kernel image to be much larger, perhaps too large to boot on computers with limited memory. This then raises the problem of detecting and loading the modules necessary to mount the root file system at boot time, or for that matter, deducing where or what the root file system is.[1]

To further complicate matters, the root file system may be on a software RAID volume, LVM, NFS (on diskless workstations), or on an encrypted partition. All of these require special preparations to mount.[2]

Another complication is kernel support for hibernation, which suspends the computer to disk by dumping an image of the entire contents of memory to a swap partition or a regular file, then powering off. On next boot, this image has to be made accessible before it can be loaded back into memory.

To avoid having to hardcode handling for so many special cases into the kernel, an initial boot stage with a temporary root file-system — now dubbed early user space — is used. This root file-system can contain user-space helpers which do the hardware detection, module loading and device discovery necessary to get the real root file-system mounted.[2]

