# Create a New Group

Create a new developer group
```
# groupadd developers
```

Validate that the group was created successfully.
```
# grep developer /etc/group
developers:x:511:
```
Add an user to an existing group
You cannot use useradd to modify an existing user, as you’ll get the following error message.
```
# useradd -G developers jsmith
useradd: user jsmith exists
# usermod -g developers jsmith
```

Validate the users group was modified successfully
```
# grep jsmith /etc/passwd
jsmith:x:510:511:Oracle Developer:/home/jsmith:/bin/bash
# id jsmith
uid=510(jsmith) gid=511(developers) groups=511(developers)
# grep jsmith /etc/group
jsmith:x:510:
developers:x:511:jsmith
```