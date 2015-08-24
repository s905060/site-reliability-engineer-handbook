# Execute Commands in the Background

#### Method 1. Use &
You can execute a command (or shell script) as a background job by appending an ampersand to the command as shown below.
```  
  $ ./my-shell-script.sh &
```

#### Method 2. Nohup
After you execute a command (or shell script) in the background using &, if you logout from the session, the command will get killed. To avoid that, you should use nohup as shown below.
```
  $ nohup ./my-shell-script.sh &
```

#### Method 3. Screen Command
After you execute a command in the background using nohup and &, the command will get executed even after you logout. But, you cannot connect to the same session again to see exactly what is happening on the screen. To do that, you should use screen command.

Linux screen command offers the ability to detach a session that is running some process, and then attach it at a later time. When you reattach the session later, your terminals will be there exactly in the way you left them earlier.

#### Method 4. At Command
Using at command you can schedule a job to run at a particular date and time. For example, to execute the backup script at 10 a.m tomorrow, do the following.
```
  $ at -f backup.sh 10 am tomorrow
```

#### Method 5. Watch Command
To execute a command continuously at a certain interval, use watch command as shown below.
```
$ watch df -h
```