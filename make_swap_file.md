# Make Swap File

Create a file for swap usage as shown below.
```
# dd if=/dev/zero of=/home/swap-fs bs=1M count=512
512+0 records in
512+0 records out

# ls -l /home/swap-fs
-rw-r--r--  1 root root 536870912 Jan  2 23:13 /home/swap-
fs
```
Use mkswap to setup a Linux swap area in the /home/swap-fs file that was created above.
```
# mkswap /home/swap-fs
```
Setting up swapspace version 1, size = 536866 kB
Once the file is created and has been setup for Linux swap area, it is time to enable the swap using swapon as shown below.