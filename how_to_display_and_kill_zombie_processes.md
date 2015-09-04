# How to display and kill zombie processes

Zombie processes are undead, scary killing them isn't so easy

### finding if zombies exist
* execute the top command
* one line is tasks:
    * Example output: Tasks: 63 total, 1 running, 61 sleeping, 0 stopped, 1 zombie

### Who is zombie
* execute:
```
ps aux | awk '"[Zz]" ~ $8 { printf("%s, PID = %d\n", $8, $2); }'
```
* example output is zombie process status, process ID: Z+, PID = 5067

### Kill the zombies
zombies are living dead, so the aren't always easy to kill.

* Try executing: kill -9 PID
example: kill -9 5067
* See is the zombie is still alive, undead
use the techniques above
* If its still undead
get a cross or garlic, well reliable sources tell me the don't work. We must try something else
* Kill the zombie's parent (process)
* execute: ps ef
    * this will display a process (family) tree
    * find the command who is the PID matches the zombie then look at the parents and try killing them

* example:
```
PID TTY      STAT   TIME COMMAND
5102 pts/3    Ss     0:00 bash MANPATH=/usr/local/share/man:/usr/share/man:/usr
5229 pts/3    R+     0:00  \_ ps ef MANPATH=/usr/local/share/man:/usr/share/man
4929 tty1     S+     0:00 -bash TERM=linux HOME=/home/zymos SHELL=/bin/bash USE
4954 tty1     S+     0:00  \_ /bin/sh /usr/bin/startx MANPATH=/usr/local/share/
4970 tty1     S+     0:00      \_ xinit /home/zymos/.xinitrc -- -nolisten tcp -
4975 tty1     S      0:01          \_ icewm MANPATH=/usr/local/share/man:/usr/s
4977 tty1     S      0:01              \_ xterm MANPATH=/usr/local/share/man:/u
4987 pts/0    Ss     0:00              |   \_ bash MANPATH=/usr/local/share/man
5090 pts/0    Sl+    1:31              |       \_ ./hydranode MANPATH=/usr/loca
5092 pts/0    Sl+    0:29              |           \_ ./hydranode-core --disabl
4978 tty1     S      0:00              \_ xterm -e /home/zymos/.icewm/startup M
4986 pts/1    Ss+    0:00              |   \_ /bin/sh /home/zymos/.icewm/startu
4993 pts/1    S+     0:00              |       \_ gaim    MANPATH=/usr/local/sh
4994 pts/1    S+     0:00              |       \_ /bin/bash /usr/libexec/mozill
5030 pts/1    Sl+    1:29              |       |   \_ /usr/lib/mozilla-firefox/
5067 pts/1    Z+     0:00              |       |       \_ [netstat] <defunct>
4995 pts/1    S+     0:00              |       \_ xterm MANPATH=/usr/local/shar
5032 pts/2    Ss+    0:00              |       |   \_ bash MANPATH=/usr/local/s
5085 pts/1    S+     0:00              |       \_ boinc_client MANPATH=/usr/loc
4981 tty1     S      0:00              \_ icewmtray MANPATH=/usr/local/share/ma
5060 pts/1    S+     0:00 /usr/libexec/gconfd-2 13 MANPATH=/usr/local/share/man
```