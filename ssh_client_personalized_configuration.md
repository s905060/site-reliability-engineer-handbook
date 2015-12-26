# SSH client personalized configuration

Under each user's HOME directory, the file is .ssh / config; configuration file can enter personalized SSH configuration, such as time-out settings

###Commonly used SSH client personalized configuration items

|Configuration Item Name	|value	|Explanation
|--|--|--|
|ConnectTimeout	|10	|Timeout Settings
|UserKnownHostsFile	|/Dev/null	|knownhosts file settings
|StrictHostKeyChecking	|no	|Whether to join a host knownhosts file links

Description: StrictHostKeyChecking no and UserKnownHostsFile /dev/null The two hosts kown key configured canceled checks for frequent reloading machine case, very useful.

###Use ProxyCommand achieve springboard Login

Scenario: If the Client is not directly pass to ServerB, and ServerA can directly reach ServerB. So by Client Configuration, Client can ssh directly to ServerB

```
 + ---------------- +      + ---------------- +      + ------------- --- + 
   |                 |      |                 |      |                 | 
   |      Client      + ----> +     ServerA      + ----> +     ServerB      | 
   |                 |      |                 |      |                 | 
   + -------------- - +      + ---------------- +      + ---------------- +
```

Suppose ServerA is 192.168.1.1 ServerB is 10.1.1.1, add the following configuration as long as the .ssh/config in
```
Host  10.1.1.1 
    ProxyCommand ssh 192.168.1.1 'nc% h% P'
```

Configuration Explanation: For host 10.1.1.1 host ssh, equivalent to execute ssh 192.168.1.1 "nc 10.1.1.1 22", although the client is not the route to 10.1.1.1
