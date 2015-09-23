# Changing the Time Zone

/etc/sysconfig/clock

The /etc/sysconfig/clock file controls the interpretation of values read from the system hardware clock.

The correct values are:

**UTC**=<value>, where <value> is one of the following boolean values:
true or yes — The hardware clock is set to Universal Time.
false or no — The hardware clock is set to local time.

**ARC**=<value>, where <value> is the following:
false or no — This value indicates that the normal UNIX epoch is in use. Other values are used by systems not supported by Red Hat Enterprise Linux.

**SRM**=<value>, where <value> is the following:
false or no — This value indicates that the normal UNIX epoch is in use. Other values are used by systems not supported by Red Hat Enterprise Linux.

**ZONE**=<filename> — The time zone file under /usr/share/zoneinfo that **/etc/localtime** is a copy of. The file contains information such as:
```
ZONE="America/New York"
```
Note that the ZONE parameter is read by the Time and Date Properties Tool (system-config-date), and manually editing it does not change the system timezone.
Earlier releases of Red Hat Enterprise Linux used the following values (which are deprecated):

CLOCKMODE=<value>, where <value> is one of the following:
GMT — The clock is set to Universal Time (Greenwich Mean Time).
ARC — The ARC console's 42-year time offset is in effect (for Alpha-based systems only).

Create a symbolic link between /etc/localtime and your time zone file so that the instance finds the time zone file when it references local time information.

```
[ec2-user ~]$ sudo ln -sf /usr/share/zoneinfo/America/Los_Angeles /etc/localtime
```