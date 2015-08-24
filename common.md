# Common

##### 1. PubkeyAuthentication option contains “yes” as the default value.
##### 2. Disable Root Login (PermitRootLogin)
##### 3. Allow Only Specific Users or Groups (AllowUsers AllowGroups)
##### 4. Deny Specific Users or Groups (DenyUsers DenyGroups)
##### 5. Change SSHD Port Number (Port)
##### 6. Change Login Grace Time (LoginGraceTime)
##### 7. Restrict the Interface (IP Address) to Login (ListenAddress)
##### 8. Disconnect SSH when no activity (ClientAliveInterval)


Find directory with 777 permission.
```
$find . -type d -perm 777
```
Check **/var/log/secure** , **/var/log/messages** and other log files of services running to see if there are any issues.

Change ssh ListenAddress /etc/ssh/sshd_config
```
#PermitRootLogin no
```

what  are the seven fields in the /etc/passwd file
```
#username, UID, GID, comment, home directory, command
```