# pstree

Linux and Unix are multitasking operating systems i.e. a system that can run multiple tasks (process) during the same period of time. A process is nothing but a running program (command) on Linux or Unix-like systems.

The pstree command shows running processes as a tree.

Display command line arguments
To see the list of command line arguments, pass the -a option:
```
$ pstree -a
```
```
init
  ├─abrt-dump-oops -d /var/spool/abrt -rwx /var/log/messages
  ├─abrtd
  ├─acpid
  ├─atd
  ├─auditd
  │   ├─audispd
  │   │   ├─sedispatch
  │   │   └─{audispd}
  │   └─{auditd}
  ├─crond
  ├─dbus-daemon --system
  │   └─{dbus-daemon}
  ├─hald
  │   ├─hald-runner
  │   │   ├─hald-addon-acpi
  │   │   └─hald-addon-inpu
  │   └─{hald}
  ├─irqbalance --pid=/var/run/irqbalance.pid
  ├─keepalived -D
  │   ├─keepalived -D
  │   └─keepalived -D
  ├─master
  │   ├─pickup -l -t fifo -u
  │   └─qmgr -l -t fifo -u
  ├─mingetty /dev/tty1
  ├─mingetty /dev/tty2
  ├─mingetty /dev/tty3
  ├─mingetty /dev/tty4
  ├─mingetty /dev/tty5
  ├─mingetty /dev/tty6
  ├─nginx
  │   ├─nginx
  │   ├─nginx
  │   └─nginx
  ├─ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
  ├─rhnsd
  ├─rhsmcertd
  ├─rsyslogd -i /var/run/syslogd.pid -c 5
  │   ├─{rsyslogd}
  │   ├─{rsyslogd}
  │   └─{rsyslogd}
  ├─sshd
  │   └─sshd
  │       └─bash
  │           └─pstree -a
  ├─udevd -d
  │   ├─udevd -d
  │   └─udevd -d
  └─vnstatd -d
```

Display PIDs
To show PIDS for each process name, pass the -p option:
```
$ pstree -p
```
```
init(1)-+-NetworkManager(1300)---{NetworkManager}(1313)
        |-accounts-daemon(2448)---{accounts-daemon}(2453)
        |-acpid(1460)
        |-aptd(6958)
        |-atd(1463)
        |-atop(3941)
        |-avahi-daemon(1359)---avahi-daemon(1361)
        |-bamfdaemon(6531)-+-{bamfdaemon}(6532)
        |                  `-{bamfdaemon}(6533)
        |-bluetoothd(1293)
        |-colord(2875)-+-{colord}(2884)
        |              `-{colord}(3091)
        |-console-kit-dae(2493)-+-{console-kit-dae}(2496)
        |                       |-{console-kit-dae}(2497)
.....
...
....
        ├─vmware-vmblock-(1792)─┬─{vmware-vmblock-}(1793)
        │                       └─{vmware-vmblock-}(1794)
        ├─whoopsie(1477)───{whoopsie}(1609)
        ├─zeitgeist-daemo(6740)───{zeitgeist-daemo}(6743)
        ├─zeitgeist-datah(6750)───{zeitgeist-datah}(6754)
        └─zeitgeist-fts(6748)─┬─cat(6756)
                              └─{zeitgeist-fts}(6755)
```