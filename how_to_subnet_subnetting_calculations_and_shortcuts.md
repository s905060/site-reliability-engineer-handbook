# How to subnet: Subnetting calculations and shortcuts

a subnet mask, break down IP address classes and understand binary. If you're wondering how to figure out what subnet mask to use when you need "x" hosts and "x" networks, this tutorial is the perfect read for you. Take the interactive quiz at the bottom of this page to find out how much you've learned.

When you are going to take a Cisco CCNA exam, before the exam starts, write the following information down (public and private class address ranges and then the chart) because it will save you a lot of time and headaches.

**IP Address Classes**
* Class A: 1 – 126 (127 reserved for loop-back and diagnostic tests)
* Class B: 128 – 191
* Class C: 192 – 223
* Class D: 224 – 239 (reserved, used primarily for IP multicasting)
* Class E: 240 – 254 (reserved, experimental)

**Private addresses**
* Class A: 10.0.0.0 through 10.255.255.255
* Class B: 172.16.0.0 through 172.31.255.255
* Class C: 192.168.0.0 through 192.168.255.255

|128	|64	|32	|16	|8	|4	|2	|1 
|--     |-- |-- |-- |-- |-- |-- |--
|1	|0	|0	|0	|0	|0	|0	|0	|128
|1	|1	|0	|0	|0	|0	|0	|0	|192
|1	|1	|1	|0	|0	|0	|0	|0	|224
|1	|1	|1	|1	|0	|0	|0	|0	|240
|1	|1	|1	|1	|1	|0	|0	|0	|248
|1	|1	|1	|1	|1	|1	|0	|0	|252
|1	|1	|1	|1	|1	|1	|1	|0	|254
|1	|1	|1	|1	|1	|1	|1	|1	|255

What the chart represents are the 8-bit values of one octet of an IPv4 IP address. (See sidebar for IPv6 subnetting resources.) If you add up all of the bits, it comes to 255 which is an all 1's mask.

If you are wondering how we get 256 in one octet, there is an implied zero at the end. If you are looking at a network you can have a network address of 192.168.17.0. The modulus of 128  0 would be 256. To arrive at the subnet mask we can do one of two things: add the positive bits together or take the least significant positive bit and subtract it from 256.