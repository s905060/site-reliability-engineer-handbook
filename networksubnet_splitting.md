# Network/Subnet splitting

* Subnet mask can be used to split a given network adress space into subnets. 

Example:
```
Original Network:  128.6.34.0 / 255.255.255.0   

can be split into 8 subnets by changing the Netmask to: 

255.255.255.224 (or 11111111.11111111.11111111.11100000) (or /27)                                         

Subnet:  128.6.34.0   / 255.255.255.224

Subnet:  128.6.34.32  / 255.255.255.224

Subnet:  128.6.34.64  / 255.255.255.224

Subnet:  128.6.34.96  / 255.255.255.224

Subnet:  128.6.34.128 / 255.255.255.224

Subnet:  128.6.34.160 / 255.255.255.224

Subnet:  128.6.34.192 / 255.255.255.224

Subnet:  128.6.34.224 / 255.255.255.224
```

* Max number of hosts on each subnet: 30 = 32 - 2
* The gateway address should be set within each subnet. 