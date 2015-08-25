# Ps

ps command (process status) will display snapshot information of all active processes.
  Syntax: ps [options]
How to display all the processes running in the system?
Use "ps aux", as shown below.
```￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼￼
# ps aux | more
USER       PID %CPU %MEM    VSZ    RSS TTY  STAT START TIME COMMAND
root         1  0.0    0.0    2044     588 ?   Ss Jun27 0:00  init [5]
apache    31186 0.0    1.6    23736  17556 ?   S  Jul26 0:40  /usr/local/apache2/bin/httpd
```