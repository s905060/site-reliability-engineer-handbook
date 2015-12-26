# How to copy your key to a remote server?

`ssh-copy-id -i username@remoteserver`

ssh-copy-id is a script that uses ssh to log into a remote machine to append your key to the authorized_keys file and sets the correct permission on ~/.ssh and ~/.ssh/authorized_keys file.

The '-i' identity option gives it the identify file (defaults to ~/.ssh/id_rsa.pub)