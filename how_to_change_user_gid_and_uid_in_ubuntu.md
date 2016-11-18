# How to change user GID and UID in Ubuntu

```bash
usermod -u <NEWUID> <LOGIN>    
groupmod -g <NEWGID> <GROUP>
find / -user <OLDUID> -exec chown -h <NEWUID> {} \;
find / -group <OLDGID> -exec chgrp -h <NEWGID> {} \;
usermod -g <NEWGID> <LOGIN>
```

usermod and groupmod simply change the UID and GID for their respective named counterpart usermod also changes the UID for the files in the homedir but naturally we can’t assume the only place files have been created is in the user’s homedir.

The find command recurses the filesystem from / and changes everything with UID of OLDUID to be owned by NEWUID and them changes the group for the files owned by the OLDGROUP.

The final usermod command changes the login group for the user.