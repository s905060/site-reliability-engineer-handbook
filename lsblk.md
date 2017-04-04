## **lsblk**

lsblk lists information about all or the specified block devices.  The lsblk command reads the sysfs filesystem to gather information.

The command prints all block devices \(except RAM disks\) in a tree-like format by default.  Use lsblk --help  to  get  a  list  of  all available columns.

If you want to check block device attributes, use [blkid command.](http://fibrevillage.com/storage/4-blkid-useful-examples)

The  default  output  as  well  as default output from options like --topology and --fs is subject to change, so whenever possible you should avoid using default outputs in your scripts. Always explicitly define expected columns by --output columns in environment where a stable output is required.

### Default output, list all block devices

It clearly shows the block devices of your system

```
$lsblk
NAME                         MAJ:MIN RM   SIZE RO TYPE   MOUNTPOINT
sda                            8:0    0 931.5G  0 disk
├─sda1                         8:1    0   500M  0 part   /boot
└─sda2                         8:2    0   931G  0 part
  ├─vg_xldesk-lv_root (dm-0) 253:0    0    50G  0 lvm    /
  ├─vg_xldesk-lv_swap (dm-1) 253:1    0  17.7G  0 lvm    [SWAP]
  └─vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdc                            8:32   0 232.9G  0 disk
└─sdc1                         8:33   0 232.9G  0 part
  └─md1                        9:1    0 232.9G  0 raid10 /data
sdb                            8:16   0 931.5G  0 disk
└─sdb1                         8:17   0 931.5G  0 part
  └─vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdd                            8:48   0 232.9G  0 disk
└─sdd1                         8:49   0 232.9G  0 part
  └─md1                        9:1    0 232.9G  0 raid10 /data
sr0                           11:0    1  1024M  0 rom
```

### lsblk with option '-a' and '-b', print size in bytes

```
   -a, --all  
          lsblk does not list empty devices by default. This option disables this restriction.  
   -b, --bytes  
          Print the SIZE column in bytes rather than in human-readable format.
```

#### Example:

    # lsblk -a -b
    NAME                      MAJ:MIN RM          SIZE RO TYPE  MOUNTPOINT
    loop0                       7:0    0                0 loop
    ...
    loop7                       7:7    0                0 loop
    sr0                        11:0    1    1073741312  0 rom
    sdb                         8:16   1  299892736000  0 disk
    |-sdb1                      8:17   1     213825024  0 part  /boot
    |-sdb2                      8:18   1    1579253760  0 part  /
    |-sdb3                      8:19   1    2155023360  0 part  [SWAP]
    |-sdb4                      8:20   1          1024  0 part
    |-sdb5                      8:21   1    4203085824  0 part  /usr
    |-sdb6                      8:22   1   26222160384  0 part  /opt
    |-sdb7                      8:23   1   26222160384  0 part  /var
    |-sdb8                      8:24   1     551061504  0 part  /tmp
    `-sdb9                      8:25   1  238736637952  0 part  /home
    sdc                         8:32   1  299891687424  0 disk
    `-sdc1                      8:33   1  299877225984  0 part  /data
    sda                         8:0    0 4806320062464  0 disk
    `-dcsunit07_lun4_5 (dm-0) 253:0    0 4806320062464  0 mpath /dcsunit07_lun4_5

### Lsblk with option '-e', '-I', '-d'

```
   -d, --nodeps  
          Don’t print device holders or slaves.  For example "lsblk --nodeps /dev/sda" prints information about the sda device only.  
   -e, --exclude list  
          Exclude the devices specified by a comma-separated list of major device numbers.  Note that RAM disks \(major=1\) are excluded by default. The filter is applied to the top-level devices only.  
   -I, --include list  
          Include devices specified by a comma-separated list of major device numbers only.  The  filter  is  applied  to  the  top-level  devices.
```

#### Example

Note: when use -e, you need to specify 1, otherwise, ram device will show up

```
# lsblk  -d -e 11,1
NAME MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sdb    8:16   1 279.3G  0 disk
sdc    8:32   1 279.3G  0 disk
sda    8:0    0   4.4T  0 disk
```

### Lsblk with option '-f', '-i', output info about filesystem

```
   -f, --fs  
          Output  info about filesystems.  This option is equivalent to "-o NAME,FSTYPE,LABEL,MOUNTPOINT".  The authoritative information about filesystems and raids is provided by the blkid\(8\) command.  
   -i, --ascii  
          Use ASCII characters for tree formatting.
```

#### Example:

    #lsblk -f -i -e 11,1
    NAME                         FSTYPE            LABEL UUID                                   MOUNTPOINT
    sda
    |-sda1                       ext4                    1a76dd6f-233b-4ff1-9b49-b930ef58a740   /boot
    `-sda2                       LVM2_member             exBAbr-UdPA-ObTp-nMCa-dJ2U-VUrm-4F9iVx
      |-vg_xldesk-lv_root (dm-0) ext4                    9026d7cf-d76a-4634-8bd9-84f34d6023f0   /
      |-vg_xldesk-lv_swap (dm-1) swap                    eda12489-4e0f-4f6b-a403-d427dafaae73   [SWAP]
      `-vg_xldesk-lv_home (dm-2) ext4                    87bab36e-c72a-48d1-a12d-4dcfc1736545   /home
    sdc
    `-sdc1                       linux_raid_member       a0d44a20-b41d-da0a-e9c5-48a1a3e98bee
      `-md1                      ext3                    957e1766-4723-420e-b942-a5dafe67c661   /data
    sdb
    `-sdb1                       LVM2_member             hTUUoY-i7R9-zAsh-CqCp-5lqp-h1qw-uU0nQV
      `-vg_xldesk-lv_home (dm-2) ext4                    87bab36e-c72a-48d1-a12d-4dcfc1736545   /home
    sdd
    `-sdd1                       linux_raid_member       a0d44a20-b41d-da0a-e9c5-48a1a3e98bee
      `-md1                      ext3                    957e1766-4723-420e-b942-a5dafe67c661   /data

### lsblk with option -l, -t

```
   -l, --list  
          Use the list output format.  
   -t, --topology  
          Output  info  about  block  device  topology.   This  option  is  equivalent  to  "-o NAME,ALIGNMENT,MIN-IO,OPT-IO,PHY-SEC,LOGSEC,ROTA,SCHED,RQ-SIZE".
```

#### Example:

```
#lsblk  -t -e 11,1
```

```
NAME                         ALIGNMENT MIN-IO OPT-IO PHY-SEC LOG-SEC ROTA SCHED RQ-SIZE   RA
sda                                  0    512      0     512     512    1 cfq       128  128
├─sda1                               0    512      0     512     512    1 cfq       128  128
└─sda2                               0    512      0     512     512    1 cfq       128  128
  ├─vg_xldesk-lv_root (dm-0)         0    512      0     512     512    1           128  128
  ├─vg_xldesk-lv_swap (dm-1)         0    512      0     512     512    1           128  128
  └─vg_xldesk-lv_home (dm-2)         0    512      0     512     512    1           128  128
sdc                                  0    512      0     512     512    1 cfq       128  128
└─sdc1                               0    512      0     512     512    1 cfq       128  128
  └─md1                              0  65536  65536     512     512    1           128  128
sdb                                  0    512      0     512     512    1 cfq       128  128
└─sdb1                               0    512      0     512     512    1 cfq       128  128
  └─vg_xldesk-lv_home (dm-2)         0    512      0     512     512    1           128  128
sdd                                  0    512      0     512     512    1 cfq       128  128
└─sdd1                               0    512      0     512     512    1 cfq       128  128
  └─md1                              0  65536  65536     512     512    1           128  128

#lsblk   -l -e 11,1
NAME                     MAJ:MIN RM   SIZE RO TYPE   MOUNTPOINT
sda                        8:0    0 931.5G  0 disk
sda1                       8:1    0   500M  0 part   /boot
sda2                       8:2    0   931G  0 part
vg_xldesk-lv_root (dm-0) 253:0    0    50G  0 lvm    /
vg_xldesk-lv_swap (dm-1) 253:1    0  17.7G  0 lvm    [SWAP]
vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdc                        8:32   0 232.9G  0 disk
sdc1                       8:33   0 232.9G  0 part
md1                        9:1    0 232.9G  0 raid10 /data
sdb                        8:16   0 931.5G  0 disk
sdb1                       8:17   0 931.5G  0 part
vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdd                        8:48   0 232.9G  0 disk
sdd1                       8:49   0 232.9G  0 part
md1                        9:1    0 232.9G  0 raid10 /data
```

### lsblk with option '-o'

custom your own output format if you want.  
       -o, --output list  
              Specify which output columns to print.  Use --help to get a list of all supported columns.

Available columns \(for --output\):

```
        NAME  device name
       KNAME  internal kernel device name
       KNAME  internal kernel device name
     MAJ:MIN  major:minor device number
      FSTYPE  filesystem type
  MOUNTPOINT  where the device is mounted
       LABEL  filesystem LABEL
        UUID  filesystem UUID
          RA  read-ahead of the device
          RO  read-only device
          RM  removable device
       MODEL  device identifier
        SIZE  size of the device
       STATE  state of the device
       OWNER  user name
       GROUP  group name
        MODE  device node permissions
   ALIGNMENT  alignment offset
      MIN-IO  minimum I/O size
      OPT-IO  optimal I/O size
     PHY-SEC  physical sector size
     LOG-SEC  logical sector size
        ROTA  rotational device
       SCHED  I/O scheduler name
     RQ-SIZE  request queue size
        TYPE  device type
    DISC-ALN  discard alignment offset
   DISC-GRAN  discard granularity
    DISC-MAX  discard max bytes
   DISC-ZERO  discard zeroes data
```



