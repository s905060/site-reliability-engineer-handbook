# Sysctl

Linux kernel parameter can be changed on the fly using sysctl command. Sysctl helps to configure the Linux kernel parameters during runtime.

```
# sysctl –a
dev.cdrom.autoclose = 1
fs.quota.writes = 0
kernel.ctrl-alt-del = 0
kernel.domainname = (none)
kernel.exec-shield = 1
net.core.somaxconn = 128
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096        16384   131072
net.ipv6.route.mtu_expires = 600
sunrpc.udp_slot_table_entries = 16
vm.block_dump = 0
```

Modify Kernel parameter in /etc/sysctl.conf for permanent change

After modifying the kernel parameter in the /etc/sysctl.conf, execute sysctl –p to commit the changes.

The changes will still be there after the reboot. Modify kernel parameter temporarily
```
# vi /etc/sysctl.conf
# sysctl –p
```
To temporarily modify a kernel parameter, execute the following command. Please note that after reboot these changes will be lost.
```
# sysctl –w {variable-name=value}
```