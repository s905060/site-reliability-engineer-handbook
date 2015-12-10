# Setting ssh private key forwarding

Set the private key through ssh forwarding to another server. 

For example A -> B -> C then A and B need to be set. 

* Make Sure ssh forwarding Works (OK ssh forwarding Works)

```$ eval `ssh-agent -s```

```# ssh-add```

```
# ps aux | grep ssh
ssh-agent is running 
```

* Ensure 

```#vim ~ / .ssh / config  add the following two lines: ServerAliveInterval 90 
ForwardAgent Yes 
```