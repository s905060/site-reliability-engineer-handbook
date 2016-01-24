# Create init script in CentOS 6

The purpose of this post is to document how to create init script in CentOS 6 to be used with the usual service management commands such as service and chkconfig.

###Introduction

After being somewhat dissatisfied with the service script provided by GlassFish, I decided to learn how to create my own service script to start, stop, and view the status of non-packaged applications.

###Learn Shell Scripting

I began by looking at other service scripts that has been already provided by CentOS such as `/etc/init.d/httpd` and `/etc/init.d/iptables` for ideas.

To help understand the scripts, I had to start learning shell scripting.  A very good tutorial can be found here.

###Writing the script

####The skeleton script

There is a file inside CentOS that explains a bit about how to write init scripts.  The path to the file is `/usr/share/doc/initscripts-*/sysvinitfiles`.  You can open this file in a text editor, and have a read.

Included in this file is a base outline of an init script that we can base our script on.  The following is the content of the outline:

```
#!/bin/bash
#
#       /etc/rc.d/init.d/<servicename>
#
#       <description of the *service*>
#       <any general comments about this init script>
#
# <tags -- see below for tag definitions.  *Every line* from the top
#  of the file to the end of the tags section must begin with a #
#  character.  After the tags section, there should be a blank line.
#  This keeps normal comments in the rest of the file from being
#  mistaken for tags, should they happen to fit the pattern.>

# Source function library.
. /etc/init.d/functions

<define any local shell functions used by the code that follows>

start() {
        echo -n "Starting <servicename>: "
        <start daemons, perhaps with the daemon function>
        touch /var/lock/subsys/<servicename>
        return <return code of starting daemon>
}

stop() {
        echo -n "Shutting down <servicename>: "
        <stop daemons, perhaps with the killproc function>
        rm -f /var/lock/subsys/<servicename>
        return <return code of stopping daemon>
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    status)
        <report the status of the daemons in free-form format,
        perhaps with the status function>
        ;;
    restart)
        stop
        start
        ;;
    reload)
        <cause the service configuration to be reread, either with
        kill -HUP or by restarting the daemons, in a manner similar
        to restart above>
        ;;
    condrestart)
        <Restarts the servce if it is already running. For example:>
        [ -f /var/lock/subsys/<service> ] && restart || :
    probe)
        <optional.  If it exists, then it should determine whether
        or not the service needs to be restarted or reloaded (or
        whatever) in order to activate any changes in the configuration
        scripts.  It should print out a list of commands to give to
        $0; see the description under the probe tag below.>
        ;;
    *)
        echo "Usage: <servicename> {start|stop|status|reload|restart[|probe]"
        exit 1
        ;;
esac
exit $?
```