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
example: