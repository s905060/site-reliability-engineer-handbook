# How to identify defective DIMM from EDAC error on Linux

DIMM error is rare, but sometime still happens. It's easy to identify them if they are completely dead, however, if a DIMM has some corrected errors, how to identify it?

I have another article listed memory testing tools on linux, this time, I use EDAC error report utility

Here is an example show you how to identify defective DIMM on an AMD_x64 archtecture machine, syslog reorted kernel error from EDAC (Error Detection and Correction kernel module).

Here is a piece of typical error message from EDAC

```
kernel: [Hardware Error]: MC4 Error (node 1): DRAM ECC error detected on the NB.
kernel: EDAC amd64 MC1: CE ERROR_ADDRESS= 0xf075b2410
kernel: EDAC MC1: CE page 0xf075b2, offset 0x410, grain 0, syndrome 0xa082, row 6, channel 0, label "": amd64_edac
kernel: [Hardware Error]: Error Status: Corrected error, no action required.
kernel: [Hardware Error]: CPU:6 (10:8:0) MC4_STATUS[-|CE|MiscV|-|AddrV|CECC]: 0x9c414000a0080813
kernel: [Hardware Error]: MC4_ADDR: 0x0000000f075b2410
kernel: [Hardware Error]: cache level: L3/GEN, mem/io: MEM, mem-tx: RD, part-proc: SRC (no timeout)
```

You may get confused by the message above, here is a quick way to show you what are they:

The structure of the message is:

```
 the memory controller                   (MC1)
 Error type                              (CE)
 memory page				             (0xf075b2)
 offset in the page                      (0x410)
 The byte granularity                    (grain 0)
 The error syndrome                      (0xb741)
 memory row                              (row 6) 
 memory channel                          (channel 0) 
 DIMM label                              Not given
 Module name                             amd64_edac
```

###More explain about info given

EDAC is composed of a "core" module (edac_core.ko) and several Memory Controller (MC) driver modules. On a given system, the CORE is loaded and one MC driver will be loaded. Both the CORE and the MC driver (or edac_device driver) have individual versions that reflect current release level of their respective modules. Thus, to "report" on what version a system is running, one must report both the CORE's and the MC driver's versions.

The example server I used in this article has these two edac module loaded:

```
# lsmod | grep -i edac
amd64_edac_mod         21913  0 
edac_core              46645  4 amd64_edac_mod
edac_mce_amd           15615  1 amd64_edac_mod
```

**Memory Controller (mc) Model**, the memory controller's model abstracted in EDAC. Each 'mc' device controls a set of DIMM memory modules. These modules are laid out in a Chip-Select Row (csrowX) and Channel table (chX).

There can be multiple csrows and multiple channels. Memory controllers allow for several csrows, with 8 csrows being a typical value.

**Channel**, each channel represents a DIMM module. Dual channels allows for 128 bit data transfers to the CPU from memory. Some system supports more channels.

**Csrow**, Chip-Select Row, shows how memory module assembled, single or dual rank or more, the actual number of csrows depends on the electrical "loading" of a given motherboard, memory controller and DIMM characteristics.

For single rank DIMM module, a pair of DIMMs merge into one csrow, typically, you will see only csrow0, while csrow1 will be empty.

See more detail about EDAC in EDAC error detection and report

Use edac-util tool to identify

See  more examples about edac-util

###Check MC info and status
```
# edac-util -vs
edac-util: EDAC drivers are loaded. 2 MCs detected:
  mc0:F10h
  mc1:F10h
```
