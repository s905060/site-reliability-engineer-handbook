# Subprogress

```
import subprocess
subprocess.check_output('ps -ef | grep something | wc -l', shell=True)
```

```
output=`dmesg | grep hda`
==>
p1 = Popen(["dmesg"], stdout=PIPE)
p2 = Popen(["grep", "hda"], stdin=p1.stdout, stdout=PIPE)
output = p2.communicate()[0]
```

```
import shlex, subprocess

command_line = raw_input()
args = shlex.split(command_line)
subprocess.check_output(args)
```

To use a pipe with the subprocess module, you have to pass shell=True.

However, this isn't really advisable for various reasons, not least of which is security. Instead, create the ps and grep processes separately, and pipe the output from one into the other, like so:

```
ps = subprocess.Popen(('ps', '-A'), stdout=subprocess.PIPE)
output = subprocess.check_output(('grep', 'process_name'), stdin=ps.stdout)
ps.wait()
```