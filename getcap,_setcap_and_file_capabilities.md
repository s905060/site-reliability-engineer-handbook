# getcap, setcap and file capabilities

Linux’s thread/process privilege checking is based on capabilities. They’re flags to the thread that indicate what kind of additional privileges they’re allowed to use. By default, root has all of these.

Examples:
CAP_DAC_OVERRIDE:
 	Override read/write/execute permission checks (full filesystem access).
CAP_DAC_READ_SEARCH:
 	Only override reading files and opening/listing directories (full filesystem READ access).
CAP_KILL:	Can send any signal to any process (such as sig kill).
CAP_SYS_CHROOT:	Ability to call chroot().

And so on.

These are useful when you want to restrict your own processes after performing privileged operations. For example, after setting up chroot and binding to a socket. (However, it’s still more limited than seccomp or SELinux, which are based on system calls instead).

### Setting/Getting capabilities from userland
You can force capabilities upon programs using setcap, and query these using getcap.

For example, on many Linux distributions you’ll find ping with cap_net_raw (which allows ping to create raw sockets). This means ping doesn’t need to run as root (via setuid, in general) anymore:
```
getcap /sbin/ping
/sbin/ping = cap_net_raw+ep
```

This has initially been set by a user with cap_setfcap (root has it by default), via this command:
```
setcap cap_net_raw+ep /sbin/ping
```
You can find the list of capabilities via:
```
man capabilities
```

The “+ep” means you’re adding the capability (“-” would remove it) as Effective and Permitted.

There are 3 modes:

The “+ep” means you’re adding the capability (“-” would remove it) as Effective and Permitted.

There are 3 modes:

* e: Effective
This means the capability is “activated”.

* p: Permitted
This means the capability can be used/is allowed.

* i: Inherited
The capability is kept by child/subprocesses upon execve() for example.

More info:
```
man cap_from_text
```