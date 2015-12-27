# How to Error Detection and Correction

Data protection and checking takes place various places throughout a system. Some of it is in hardware and some of it is in software. The goal is to ensure that data is not corrupted (changed), either coming from or going to the hardware or in the software stack. One key technology is ECC memory (error-correcting code memory).

The standard ECC memory used in systems today can detect and correct what are called single-bit errors, and although it can detect double-bit errors, it cannot correct them. A simple flip of one bit in a byte can make a drastic difference in the value of the byte. For example a byte (8 bits) with a value of 156 (10011100) that is read from a file on disk suddenly acquires a value of 220 if the second bit from the left is flipped from a 0 to a 1 (11011100) for some reason.

ECC memory can detect the problem and correct it so with the user unaware. Notice, however, that only one bit in the byte has been changed and then corrected. If two bits change – perhaps by both the second and seventh from the left – the byte is now 11011110 (i.e., 222); typical ECC memory can detect that the “double-bit” error occurred, but it cannot correct it. In fact, when a double-bit error happens, memory should cause what is called a “machine check exception” (mce), which should cause the system to crash. After all, you are using ECC memory, so ensuring the data is correct is important; if an uncorrectable memory error occurs, you would probably want the system to stop.

The source of bit-flipping usually originates in some sort of electrical or magnetic interference inside the system. This interference can cause a bit to flip at seemingly random times, depending on the circumstances. According to the Wikipedia article and a paper on single-event upsets in RAM, most single-bit flips are the result of background radiation – primarily neutrons from cosmic rays.

The same Wikipedia article reports that the error rates reported from 2007 to 2009 varied all over the map, ranging from 10–10 (errors/bit-hr) to 10–17 (seven orders of magnitude difference). The lower number is just about one error per gigabit of memory per hour. The upper number indicates roughly one error every 1,000 years per gigabit of memory.

A study of real memory errors took place at Google. During their investigations they found that one third of the machines and more than 8 percent of the DIMMs saw correctable errors per year. This translates to Google experiencing about 25,000–75,000 correctable errors (CE) per billion device hours per megabit, which translates to 2,000–6,000 CE/GB-yr (or about 250–750 CE/Gb-yr). This is much higher than the previously reported “high” correctable error rate of 1 CE/Gb-yr (250–750 times higher) and six orders of magnitude higher than the optimistic report.

The study went on to report some other interesting results that bear repeating here.

* Memory Errors are strongly correlated
 * There is a strong correlation among correctable errors within the same DIMM. A DIMM that has a correctable error is 13–228 times more likely to see another in the same month.
 * An uncorrectable error is preceded by a correctable error 70–80 percent of the time. A correctable error increases the probability of an uncorrectable error by factors of 9–400.
 * Uncorrectable errors following a correctable error are still small at 0.1%–2.3% per year.
* The incidence of correctable errors increases with age, but the incidence of uncorrectable errors decreases with age
 * The increasing incidence of correctable errors sets in after about 10–18 months.
 * The most likely reason for uncorrectable errors decreasing is that DIMMs with a large number of correctable errors are replaced, decreasing the likelihood of uncorrectable errors.
* There is no evidence that newer generationDIMMs have worse behavior (this study was published in 2009)
* Temperature had a surprisingly low effect on memory errors (over the temperature range tested)
* Error rates are strongly correlated with utilization
* Error rates are unlikely to be dominated by software errors

To me, these observations indicate that in real-world production, we see much higher error rates than what manufacturers are reporting. Moreover, the rate of correctable errors can be an important factor in watching for memory failure. Consequently, I think monitoring and capturing the correctable error information is very important.

###Linux and Memory Errors

When I worked for Linux Networx years ago, they were helping with a project that was called bluesmoke. The idea was to have a kernel module that could catch and report hardware-related errors within the system. This goes beyond just memory errors to include hardware errors in the cache, DMA, fabric switching, thermal throttling, hypertransport bus, and so on. The formal name of the project was EDAC, Error Detection and Correction.

For many years, people wrote EDAC kernel modules for various chipsets so they could capture hardware-related error information and report it. This was initially done outside the kernel at the beginning of the project, but, starting with kernel 2.6.16 (released March 20, 2006), edac was included with the kernel. Starting with kernel 2.6.18, EDAC showed up in the /sys file system, typically in /sys/devices/system/edac .

One of the best sources of information about EDAC can be found at the EDAC wiki. The page discusses how to get started and is also a good location for EDAC resources (bugs, FAQs, mailing list, etc.).

Rather than focus on getting EDAC working, I want to focus on what information it can provide and why it is important. I'll be using a Dell PowerEdge R720 as an example system. It has two processors (Intel E5-2600 series) and 128GB of ECC memory. It was running CentOS 6.2 during the tests.

For the test system, I checked to see whether any EDAC modules were loaded with lsmod :

```
login2$ /sbin/lsmod
...
sb_edac 12898 0
edac_core 46773 3 sb_edac
...
```

EDAC was loaded as a module, so I examined the directory /sys/devices/system/edac :
```
login2$ ls -s /sys/devices/system/edac/
total 0
0 mc
```

Because I can only see the mc devices, EDAC is only monitoring the memory controller(s). If I probe a little further,
```
login2$ ls -s /sys/devices/system/edac/mc
total 0
0 mc0 0 mc1
```

I find two EDAC components, mc (memory controllers), for this system.
Peering into mc0 shows the following:
```
login2$ ls -s /sys/devices/system/edac/mc/mc0
total 0
0 ce_count 0 csrow1 0 csrow4 0 csrow7 0 reset_counters 0 size_mb
0 ce_noinfo_count 0 csrow2 0 csrow5 0 device 0 sdram_scrub_rate 0 ue_count
0 csrow0 0 csrow3 0 csrow6 0 mc_name 0 seconds_since_reset 0 ue_noinfo_count
```

Notice that this system has two memory controllers, mc0 and mc1 , which control a number of DIMMs. These DIMMs are laid out in a “chip-select” row (csrow ) and a channel table (chx ) (see the EDAC documentation for more details). There can be multiple csrow values and multiple channels. For example, here is a simple ASCII sketch of two csrows and two channels.

```
Channel 0 Channel 1
==============================
csrow0 | DIMM_A0 | DIMM_B0 |
csrow1 | DIMM_A0 | DIMM_B0 |
==============================
==============================
csrow2 | DIMM_A1 | DIMM_B1 |
csrow3 | DIMM_A1 | DIMM_B1 |
==============================
```

The number of csrows depends on the electrical loading of a given motherboard and the memory controller and DIMM characteristics.

**Example**

For this example node, each memory controller has eight csrows and one channel table. You can get an idea of the layout by looking at the entries for csrowX (X = 0 to 7):
```
login2$ more /sys/devices/system/edac/mc/mc0/csrow0/ch0_dimm_label
CPU_SrcID#0_Channel#0_DIMM#0
login2$ more /sys/devices/system/edac/mc/mc0/csrow1/ch0_dimm_label
CPU_SrcID#0_Channel#0_DIMM#1
login2$ more /sys/devices/system/edac/mc/mc0/csrow2/ch0_dimm_label
CPU_SrcID#0_Channel#1_DIMM#0
login2$ more /sys/devices/system/edac/mc/mc0/csrow3/ch0_dimm_label
CPU_SrcID#0_Channel#1_DIMM#1
login2$ more /sys/devices/system/edac/mc/mc0/csrow4/ch0_dimm_label
CPU_SrcID#0_Channel#2_DIMM#0
login2$ more /sys/devices/system/edac/mc/mc0/csrow5/ch0_dimm_label
CPU_SrcID#0_Channel#2_DIMM#1
login2$ more /sys/devices/system/edac/mc/mc0/csrow6/ch0_dimm_label
CPU_SrcID#0_Channel#3_DIMM#0
login2$ more /sys/devices/system/edac/mc/mc0/csrow7/ch0_dimm_label
CPU_SrcID#0_Channel#3_DIMM#1
```

This information shows four memory channels (0–3) and two DIMMs for each channel (0 and 1).

Each csrow subdirectory for each memory controller has several EDAC control and attribute files for that csrow. For example, the output for mc0/csrow0 ,

```
login2$ ls -s /sys/devices/system/edac/mc/mc0/csrow0
total 0
0 ce_count 0 ch0_dimm_label 0 edac_mode 0 size_mb
0 ch0_ce_count 0 dev_type 0 mem_type 0 ue_count
```

shows that all are files (no further subdirectories). The definition of each file is:

* ce_count : The total count of correctable errors that have occurred on this csrow (attribute file).
* ch0_ce_count : The total count of correctable errors on this DIMM in channel 0 (attribute file).
* ch0_dimm_label : The control file that labels this DIMM. This can be very useful for panic events to isolate the cause of the uncorrectable error. Note that DIMM labels must be assigned after booting, with information that correctly identifies the physical slot with its silk screen label on the board itself.
* dev_type : An attribute file that will display the type of DRAM device being used on this DIMM. Typically this is x1 , x2 , x4 , or x8 .
* edac_mode : An attribute file that displays the type of error detection and correction being utilized.
* mem_type : An attribute file that displays the type of memory currently on a csrow.
* size_mb : An attribute file that contains the size (MB) of memory a csrow contains.
* ue_count : An attribute file that contains the total number of uncorrectable errors that have occurred on a csrow.