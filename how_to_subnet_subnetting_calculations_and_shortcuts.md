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

Where the chart comes in handy is if you are being asked what the subnet mask would be for /27. You know that the sum of the first three octets is 24 so you start counting 25 with the first bit (128). Counting to 27 would get you to the third most significant bit (32). Look to the subnet mask column and you see that it is .224.

I had a teacher -- Jeremy Cioara, who has written several Cisco books --who told a story about a guy who owned a brick company. He had  a lot of bricks, and people would come and buy bricks. As the guy got more successful, he had to figure out a way of putting the bricks in piles so it would be quick and easy to give them the number of bricks they wanted. He then put the bricks on pallets in the following quantities: 128, 64, 32, 16, 8, 4, 2 and 1. As a result, it was easy to come up with quantities for a customer. For example, if a customer wants 188 bricks, you would give them one of the 128 pallets. They are 60 bricks short. You sure as heck are not going to give him a 64-brick pallet because you would lose money so you give him a 32-brick pallet. You are now 28 bricks short so you give him a 16-brick pallet. You are still 12 bricks short so you give him an 8-brick pallet and a 4-brick pallet. Here is the binary of 188:

128	64	32	16	8	4	2	1
1	0	1	1	1	1	0	0

Jeremy used this method to decide what subnet mask to use when you need "x" hosts and "x" networks.

I had another teacher who used to do the 2 ⁿ -2 table. That's a little too complicated when you are going against time in a Cisco exam.

Here's how I determine networks/hosts. You know that you have to subtract two because you need one end for the network and one end for the broadcast address (subnet mask). So, when you get to a bit value (i.e., 128, 64, 32 etc.) you subtract two and the remainder will be the hosts.

Let's say that you are told that you have 57 hosts and are asked for the subnet mask. Easy. You go to the chart and find that 57 is more than 32 but less than 64. Try to think 30 and 62 because you are thinking hosts. You would use the second bit, 64, and so if you go to the right side for the subnet mask it would be .192.

If you are asked for valid hosts in the 192.168.30.7/28 range, you start out by counting off the chart to see the numerical value for the 28th bit. It is 16. Then take a look at the last octet (since 28 puts us in the last octet range) and divide it by 16. In this case, we don't have to because we can see it is less than 16 but if you are given .189 rather than .7 you would divide 189 by 16 and see that it was over 11. Then multiply 11 times 16 to see the first address in that subnet -- 176. Then add 16 to 176 and you will get the first address of the next subnet (192). That means that .189 falls in the subnet 176  188. We know that the first number is the network address (192.168.30.176), and the last number is the broadcast address (255.255.255.188). Remember that we are breaking down that 256 block so we can't use the full 256 subnet, only the portion blocking our network). That means that the hosts must fall in the range of 177  187 (one after 176 and one before 188).

Going back to our question of valid hosts in the 192.168.30.7/28 range they would be:

192.168.30.1
192.168.30.2
192.168.30.3
192.168.30.4
192.168.30.5
192.168.30.6
192.168.30.7
192.168.30.8
192.168.30.9
192.168.30.10
192.168.30.11
192.168.30.12
192.168.30.13
192.168.30.14

If you counted them up, you would see that there are 14 of them. Remember the 2 ⁿ -2 formula -- the bit value minus 2 (one for network and one for broadcast address or subnet mask) equals the number you can use for hosts.

If you want to know how many of these 16-bit networks we can get out of that third octet, divide 256 by 16. It is 16. Don't slip and think that the number of networks you can get is the bit value -- it just works out that way with /28. For a /26 you can get four networks (0  63; 64  127; 128  191 and 192  255 (I didn't use the number 256 because I started at zero -- the modulus (value of 256) comes from zero to 255).