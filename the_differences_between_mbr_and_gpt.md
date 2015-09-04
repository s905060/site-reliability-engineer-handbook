# The Differences Between MBR and GPT

If you have dabbled with your hard disk and is always doing formatting and partitioning, you will surely come across the term “MBR” and “GPT”. This is especially evident when you are dual-booting your Mac and faced with the problem of having to switch from GPT to MBR. You probably are wondering, what are the differences between MBR and GPT and is there any benefit using one over the other? We wil clear your doubt in this article.

### Hard Disk Partitions

You probably know that you can split your hard disk into several partitions. The question is, how does the OS know the partition structure of the hard disk? That information has to come from some where. This is where MBR (Master Boot Record) and GPT (Guid Partition Table) come into play. While both are architecturally different, both play the same role in governing and provide information for the partitions in the hard disk.

### Master Boot Record (MBR)
[mte_content_ads]MBR is the old standard for managing the partition in the hard disk, and it is still being used extensively by many people. The MBR resides at the very beginning of the hard disk and it holds the information on how the logical partitions are organized in the storage device. In addition, the MBR also contains executable code that can scan the partitions for the active OS and load up the boot up code/procedure for the OS.

For a MBR disk, you can only have four primary partitions. To create more partitions, you can set the fourth partition as the extended partition and you will be able to create more sub-partitions (or logical drives) within it. As MBR uses 32-bit to record the partition, each partition can only go up to a maximum of 2TB in size. This is how a typical MBR disk layout looks like: