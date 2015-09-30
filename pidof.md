# Pidof

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