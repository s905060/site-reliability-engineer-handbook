# How to Write Linux Init Scripts Based on LSB Init Standard

LSB stands for Linux Standard Base.

LSB was started by Linux Foundation to reduce the difference between several Linux distributions, and thereby reducing the cost involved in porting between different distributions. Init scripts are one among them to be standardized.

In this article, we will see how to write an Init script that conforms to LSBInit Standard.

Init scripts are used to start|stop a software|service. For example, if you are using postgresql software, we will have a Init script named ‘/etc/init.d/postgresql’ which can be used to ‘start|stop|restart|reload|force-reload|status’.

LSB-compliant init scripts need to:

* Provide at-least ‘start, stop, restart, force-reload, and status’
* Return Proper exit code
* Document run-time dependencies

Optionally, they can use init.d functions like “log_success_msg”, “log_failure_msg” etc.. to log the messages.

LSB provides default set of functions which is in /lib/lsb/init-functions. We can make use of those functions in our Init scripts. All the Init process by default will log the pid of the process in a file under /var/run/ directory. This is useful for the Init scripts to find the status of the process.

Now let’s start writing a Init script. First we need to add a header to the Init script, which looks like,

```shell
### BEGIN INIT INFO
# Provides:          my_daemon
# Required-Start:    postgresql networking
# Required-Stop:     postgresql networking
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: This is a test daemon
# Description:       This is a test daemon
#                    This provides example about how to
#                    write a Init script.
### END INIT INFO
```

**Provides** specifies what is the facility provided by this Init script. The name should be unique.

**Required-start** specifies the set of facilities that should be started before starting this service. In our case, postgresql and networking has to be started before starting my_daemon. Remember, here ‘postgresql’ will have a separate Init script which says ‘Provides: postgresql’. This ensures dependency based booting. Based on this dependency, the ‘inserv’ command or ‘update-rc.d’ will put the entries in run-level directories with appropriate sequence number. For example, the entries can be like follows

```shell
/etc/rc2.d/S12postgresql
/etc/rc2.d/S03networking
/etc/rc2.d/S13my_daemon
```

Which indicates, networking will be started first, then postgresql and then my_daemon.

To know more about run-levels, refer to stage#6 (which is runlevel) of Linux boot process.

**Required-Stop** specifies the list of facilities that has to be stopped only after stopping this facility. Here only after my_daemon is stopped, the postgresql and networking facilities will be stopped.

**Default-Start** Default-Stop defines the run-levels in which the service has to be started or stopped.

**Short-Description** and **Description** are used to give some description with regard the to the facility provided. Description can span across multiple lines, Short-Description is limited to single line.

Let’s look at an actual Init script.