# Pidof

pidof = find the process ID of a running program

Pidof finds the process id's (pids) of the named programs. It prints those id's on the standard output. This program is on some systems used in run-level change scripts, especially when the system has a System-V like rc structure.
```
sysadmin@codewarden:~$ pidof apache2
5098 5095 5094 5092
```

pgrep = look up or signal processes based on name and other attributes, pgrep looks through the currently running processes and lists the process IDs which matches the selection criteria.
```
sysadmin@codewarden:~$ pgrep apache2
5092
5094
5095
5098
```
pgrep, (p) = process, grep = grep prints the matching lines

Want to know more about pgrep & pidof ? Just run in terminal as

\# man pidof

\# man pgrep

How-To: Kill a Process Using the 'pidof' Command
If a process hangs and you want to easily kill it, type in a console:

```
kill -9 $(pidof process_name)
```

And replace process_name with a currently running process. For example, to kill Amarok you would issue the following:
```
kill -9 $(pidof amarokapp)
```
Or Banshee, as another example:
```
kill -9 $(pidof banshee)
```
Or
```
kill -9 $(pidof banshee-1)
```
pidof is a command that finds the process ID (PID) of a given application. What is inside the ( and ) parenthesis is replaced with a certain PID, and the process which has that PID will be killed.