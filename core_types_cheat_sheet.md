# Core Types Cheat Sheet

###THE TRIFECTA
Package/file/service: Learn it, live it, love it. If you can
only do this, you can still do a lot.

```
package { 'openssh-server':
    ensure => installed,
}
file { '/etc/ssh/sshd_config':
    source => 'puppet:///modules/sshd/
sshd_config',
    owner => 'root',
    group => 'root',
    mode => '640',
    notify => Service['sshd'], # sshd
              will restart whenever you
              edit this file.
    require => Package['openssh-server'],
}
service { 'sshd':
    ensure => running,
    enable => true,
    hasstatus => true,
    hasrestart => true,
}
```

![](Screen Shot 2015-12-31 at 3.15.38 AM.png)

###file
Manages local files

**ATTRIBUTES**
* ensure — Whether the file should exist, and what it
should be.
* present
* absent
* file
* directory
* link
* path — The fully qualified path to the file; defaults
to title.
* source — Where to download the file. A puppet:///
URL to a file on the master, or a path to a local file on
the agent.
* content — A string with the file’s desired contents.
Most useful when paired with templates, but you can
also use the output of the file function.
* target — The symlink target. (When ensure => link.)
* recurse — Whether to recursively manage the
directory. (When ensure => directory.)
* true or false
* purge — Whether to keep unmanaged files out of the
directory. (When recurse => true.)
* true or false
* owner — By name or UID.
* group — By name or GID.
* mode — Must be specified exactly. Does the right thing
for directories.
* See also: backup, checksum, force, ignore,
links, provider, recurselimit, replace,
selrange, selrole, seltype, seluser,
sourceselect, type.