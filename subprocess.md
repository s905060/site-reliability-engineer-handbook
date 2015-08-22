# Subprogress

```
import shlex, subprocess

command_line = raw_input()
args = shlex.split(command_line)
subprocess.check_output(args)
```