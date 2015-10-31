# HOWTO: Use Wireshark over SSH

What you need:

Source system (the server you want to capture packets on) that you have SSH access to, with tcpdump installed, and available to your user (either directly, or via sudo without password).
Destination system (where you run graphical Wireshark) with wireshark installed and working, and mkfifo available.
Procedure:

On the destination system, if you haven’t already done so,
```
mkfifo /tmp/packet_capture
```

This creates a named pipe where the source packet data (via ssh) will be written and Wireshark will read it from. You can use any name or location you want, but /tmp/packet_capture is pretty logical.

On your destination system, open up Wireshark (we do this now, since on many systems it required the root password to start). In the “Capture” menu, select “Options”. In the “Interface” box, type in the path to the FIFO you created (/tmp/packet_capture). You should press the Start button before running the next command - I recommend typing the command in a terminal window, pressing start, then hitting enter in the terminal to run the command.

On the destination system, run

```
ssh user@source-hostname "sudo /usr/sbin/tcpdump -s 0 -U -n -w - -i eth0 not port 22" > /tmp/packet_capture
```

This will SSH to the source system (source-hostname, either by hostname or IP) as the specified user (user) and execute sudo /usr/sbin/tcpdump. Omit the “sudo” if you don’t need it, though if you do, you’ll need passwordless access. Options passed to tcpdump are: “-s 0” snarf entire packets, no length limit; “-U” packet-buffered output - write each complete packet to output once it’s captured, rather than waiting for a buffer to fill up; “-n” don’t convert addresses to hostnames; “-w -” write raw packets to STDOUT (which will be passed through the SSH tunnel and become STDOUT of the “ssh” command on the destination machine); “-i eth0” capture on interface eth0; “not port 22” a tcpdump filter expression to prevent capturing our own SSH packets (more on this below). The final “> /tmp/packet_capture” redirects the STDOUT of the ssh program (the raw packets from tcpdump on the source machine) to the /tmp/packet_capture FIFO.

When you’re ready to stop the capture, just Ctrl+C the SSH command in the terminal window. Wireshark will automatically stop capturing, and you can save the capture file or play around with it. To capture again, you’ll need to restart the capture in Wireshark and then run the ssh command again.

A note on network usage and tcpdump filters

This is a relatively bandwidth intensive procedure. If you use the “not port 22” tcpdump filter (shown above) on the source machine, all traffic over eth0 (other than SSH) on that machine will be duplicated within an SSH tunnel. So you have double the traffic, plus the overhead of tunneling all that within SSH to the destination machine. If you’re capturing data from a busy machine this way, you could easily saturate the uplink and wreak all sorts of havoc. As a result, I’d recommend making the tcpdump filter as specific as you can while still retaining the data you need. If you can replace it with a filter for specific ports (i.e. '(port 67 or port 68)' for DHCP) or specific hosts, that should cut down on the amount of data you actually have to pass through the tunnel.




Linux

```
ssh remote-host "tcpdump -s0 -w - 'port 8080'" | wireshark -k -i -
```

This will run tcpdump on host “remote-host” and capture full packages (-s0) on port 8080. The output is sent over SSH to the local host’s “stdout” where Wireshark is waiting on “stdin” for input. (-k means start immediately).

There are a few things that may make the line above not work in your case. Make sure tcpdump is on the path on your remote host or change the line to include the path a la:

```
ssh remote-host "/usr/sbin/tcpdump -s0 -w - 'port 8080'" | wireshark -k -i -
```

You may also need to run tcpdump with sudo which means you need to change the command to:

```
ssh remote-host "sudo /usr/sbin/tcpdump -s0 -w - 'port 8080'" | wireshark -k -i -
```

Please note! Such a remote capture session can be pretty heavy on the network depending on the application. Make sure you filter as much as possible on the remote side using tcpdump’s filters.