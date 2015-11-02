# ethtool

Ethtool utility is used to view and change the ethernet device parameters.

### 1. List Ethernet Device Properties

When you execute ethtool command with a device name, it displays the following information about the ethernet device.
```
# ethtool eth0
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Advertised auto-negotiation: Yes
        Speed: 100Mb/s
        Duplex: Full
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: on
        Supports Wake-on: d
        Wake-on: d
        Link detected: yes
```

This above ethtool output displays ethernet card properties such as speed, wake on, duplex and the link detection status. Following are the three types of duplexes available.

Full duplex : Enables sending and receiving of packets at the same time. This mode is used when the ethernet device is connected to a switch.
Half duplex : Enables either sending or receiving of packets at a single point of time. This mode is used when the ethernet device is connected to a hub.
Auto-negotiation : If enabled, the ethernet device itself decides whether to use either full duplex or half duplex based on the network the ethernet device attached to.

###2. Change NIC Parameter Using ethtool Option -s autoneg

The above ethtool eth0 output displays that the “Auto-negotiation” parameter is in enabled state. You can disable this using autoneg option in the ethtool as shown below.
```
# ifdown eth0
    eth0      device: Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet (rev 20)
    eth0      configuration: eth-bus-pci-0000:0b:00.0

# ethtool  -s eth0 autoneg off

# ethtool eth0
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  Not reported
        Advertised auto-negotiation: No
        Speed: Unknown! (65535)
        Duplex: Unknown! (255)
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: off
        Supports Wake-on: g
        Wake-on: g
        Link detected: no
# ifup eth0
```

After the above change, you could see that the “link detection” value changed to down and auto-negotiation is in off state.

### 3. Change the Speed of Ethernet Device

Using ethtool you can change the speed of the ethernet device to work with the certain network devices, and the newly assign speed value should be within the limited capacity.

```
# ethtool -s eth0 speed 100 autoneg off

# ethtool eth0
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  Not reported
        Advertised auto-negotiation: No
        Speed: Unknown! (65535)
        Duplex: Unknown! (255)
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: off
        Supports Wake-on: g
        Wake-on: g
        Link detected: no
```

Once you change the speed when the adapter is online, it automatically goes offline, and you need to bring it back online using ifup command.
```
# ifup eth0
    eth0      device: Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet (rev 20)
    eth0      configuration: eth-bus-pci-0000:0b:00.0
Checking for network time protocol daemon (NTPD):                     running

# ethtool eth0
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  Not reported
        Advertised auto-negotiation: No
        Speed: 100Mb/s
        Duplex: Full
        Port: Twisted Pair
        PHYAD: 1
        Transceiver: internal
        Auto-negotiation: off
        Supports Wake-on: g
        Wake-on: g
        Link detected: yes
```
As shown in the above output, the speed changed from 1000Mb/s to 100Mb/s and auto-negotiation parameter is unset.

To change the Maximum Transmission Unit (MTU), refer to our ifconfig examples article.

### 4. Display Ethernet Driver Settings

ethtool -i option displays driver version, firmware version and bus details as shown below.

```
# ethtool -i eth0
driver: bnx2
version: 2.0.1-suse
firmware-version: 1.9.3
bus-info: 0000:04:00.0
```

### 5. Display Auto-negotiation, RX and TX of eth0

View the autonegotiation details about the specific ethernet device as shown below.

```
# ethtool -a eth0
Pause parameters for eth0:
Autonegotiate:  on
RX:             on
TX:             on
```

###6. Display Network Statistics of Specific Ethernet Device

Use ethtool -S option to display the bytes transfered, received, errors, etc, as shown below.

```
# ethtool -S eth0
NIC statistics:
     rx_bytes: 74356477841
     rx_error_bytes: 0
     tx_bytes: 110725861146
     tx_error_bytes: 0
     rx_ucast_packets: 104169941
     rx_mcast_packets: 138831
     rx_bcast_packets: 59543904
     tx_ucast_packets: 118118510
     tx_mcast_packets: 10137453
     tx_bcast_packets: 2221841
     tx_mac_errors: 0
     tx_carrier_errors: 0
     rx_crc_errors: 0
     rx_align_errors: 0
     tx_single_collisions: 0
     tx_multi_collisions: 0
     tx_deferred: 0
     tx_excess_collisions: 0
     tx_late_collisions: 0
     tx_total_collisions: 0
     rx_fragments: 0
     rx_jabbers: 0
     rx_undersize_packets: 0
     rx_oversize_packets: 0
     rx_64_byte_packets: 61154057
     rx_65_to_127_byte_packets: 55038726
     rx_128_to_255_byte_packets: 426962
     rx_256_to_511_byte_packets: 3573763
     rx_512_to_1023_byte_packets: 893173
     rx_1024_to_1522_byte_packets: 42765995
     rx_1523_to_9022_byte_packets: 0
     tx_64_byte_packets: 3633165
     tx_65_to_127_byte_packets: 51169838
     tx_128_to_255_byte_packets: 3812067
     tx_256_to_511_byte_packets: 113766
     tx_512_to_1023_byte_packets: 104081
     tx_1024_to_1522_byte_packets: 71644887
     tx_1523_to_9022_byte_packets: 0
     rx_xon_frames: 0
     rx_xoff_frames: 0
     tx_xon_frames: 0
     tx_xoff_frames: 0
     rx_mac_ctrl_frames: 0
     rx_filtered_packets: 14596600
     rx_discards: 0
     rx_fw_discards: 0
```

### 7. Troubleshoot the Ethernet Connection Issues

When there is a problem with the network connection, you might want to check (or change) the ethernet device parameters explained in the above examples, when you see following issues in the output of ethtool command.

Speed and Duplex value is shown as Unknown
Link detection value is shown as No
Upon successful connection, the three parameters mentioned above gets appropriate values. i.e Speed is assigned with known value, Duplex become either Full/Half, and the Link detection becomes Yes.

After the above changes, if the Link Detection still says “No”, check whether there are any issues in the cables that runs from the switch and the system, you might want to dig into that aspect further.

To capture and analyze packets from a specific network interface, use tcpdump utility.

### 8. Identify Specific Device From Multiple Devices (Blink LED Port of NIC Card)

Let us assume that you have a machine with four ethernet adapters, and you want to identify the physical port of a particular ethernet card. (For example, eth0).

Use ethtool option -p, which will make the corresponding LED of physical port to blink.
```
# ethtool -p eth0
```

### 9. Make Changes Permanent After Reboot

If you’ve changed any ethernet device parameters using the ethtool, it will all disappear after the next reboot, unless you do the following.

On ubuntu, you have to modify /etc/network/interfaces file and add all your changes as shown below.
```
# vim /etc/network/interfaces
post-up ethtool -s eth2 speed 1000 duplex full autoneg off
```

The above line should be the last line of the file. This will change speed, duplex and autoneg of eth2 device permanently.

On SUSE, modify the /etc/sysconfig/network/ifcfg-eth-id file and include a new script using POST_UP_SCRIPT variable as shown below. Include the below line as the last line in the corresponding eth1 adpater config file.
```
# vim /etc/sysconfig/network/ifcfg-eth-id
POST_UP_SCRIPT='eth1'
Then, create a new file scripts/eth1 as shown below under /etc/sysconfig/network directory. Make sure that the script has execute permission and ensure that the ethtool utility is present under /sbin directory.
```
```
# cd /etc/sysconfig/network/

# vim scripts/eth1
#!/bin/bash
/sbin/ethtool -s duplex full speed 100 autoneg off
```