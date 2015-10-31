# HOWTO: Use Wireshark over SSH

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