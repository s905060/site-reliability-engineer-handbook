# How to check what sudo acces a user has?

`sudo -l -U bob`

#How to Add groups/users to /etc/sudoers?

This is the recommended approach as it avoides polluting the original sudo file and makes management & maintenance easy.

Instead of modifying the /etc/sudoers file do the following:

Create a /etc/sudoers.d dir if it does not exist
Create a file with no extension and file permission 0440 (do this outside of the /etc/sudoers.d dir)
```
chmod 0440 <filename>
chown root:root <filename>
```

Add the file to /etc/sudoers.d/ dir.
Include the directory in the sudoers file by performing:

```
# /usr/sbin/visudo sudo visudo

# Add the following to the sudoers file. Note the hash has been explicitly added: #includedir /etc/sudoers.d
```

All files now added to/etc/sudoers.d` will be automatically imported

