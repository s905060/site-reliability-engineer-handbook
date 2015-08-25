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
