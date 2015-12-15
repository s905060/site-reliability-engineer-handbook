# SSH ProxyCommand example: Going through one host to reach another server

Edit local `.ssh/config`

```
Host gateway
    HostName FQDN of IP address
    User Your User Name
    
Host RemoteInternalHost
    HostName FQDN of IP address
    User Your User Name
    ProxyCommand ssh -NW %h:%p gateway

Host *
    ServerAliveInterval 999
    ForwardAgent yes
    StrictHostKeyChecking no
```