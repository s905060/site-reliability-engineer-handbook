# Persist keys in ssh-agent on OS X

On OS X you can persist keys in ssh-agent by storing them in the built in "Keychain" application by using the -K option to ssh-add

```
ssh-add -K /path/to/key
```

Be aware there are some special caveats if you're using password protected SSH keys from older installations of OpenSSH (like on a previous version of OS X) when doing this on Mavericks. This Stack Exchange question covers the issue and has a solution.