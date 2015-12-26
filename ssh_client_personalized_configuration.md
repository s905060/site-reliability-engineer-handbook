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