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

```bash
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

```bash
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

```bash
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

# Using the lsb functions to perform the operations.
. /lib/lsb/init-functions
# Process name ( For display )
NAME=my-daemon
# Daemon name, where is the actual executable
DAEMON=/home/user1/my_daemon
# pid file for the daemon
PIDFILE=/var/run/my_daemon.pid

# If the daemon is not there, then exit.
test -x $DAEMON || exit 5

case $1 in
 start)
  # Checked the PID file exists and check the actual status of process
  if [ -e $PIDFILE ]; then
   status_of_proc -p $PIDFILE $DAEMON "$NAME process" && status="0" || status="$?"
   # If the status is SUCCESS then don't need to start again.
   if [ $status = "0" ]; then
    exit # Exit
   fi
  fi
  # Start the daemon.
  log_daemon_msg "Starting the process" "$NAME"
  # Start the daemon with the help of start-stop-daemon
  # Log the message appropriately
  if start-stop-daemon --start --quiet --oknodo --pidfile $PIDFILE --exec $DAEMON ; then
   log_end_msg 0
  else
   log_end_msg 1
  fi
  ;;
 stop)
  # Stop the daemon.
  if [ -e $PIDFILE ]; then
   status_of_proc -p $PIDFILE $DAEMON "Stoppping the $NAME process" && status="0" || status="$?"
   if [ "$status" = 0 ]; then
    start-stop-daemon --stop --quiet --oknodo --pidfile $PIDFILE
    /bin/rm -rf $PIDFILE
   fi
  else
   log_daemon_msg "$NAME process is not running"
   log_end_msg 0
  fi
  ;;
 restart)
  # Restart the daemon.
  $0 stop && sleep 2 && $0 start
  ;;
 status)
  # Check the status of the process.
  if [ -e $PIDFILE ]; then
   status_of_proc -p $PIDFILE $DAEMON "$NAME process" && exit 0 || exit $?
  else
   log_daemon_msg "$NAME Process is not running"
   log_end_msg 0
  fi
  ;;
 reload)
  # Reload the process. Basically sending some signal to a daemon to reload
  # it configurations.
  if [ -e $PIDFILE ]; then
   start-stop-daemon --stop --signal USR1 --quiet --pidfile $PIDFILE --name $NAME
   log_success_msg "$NAME process reloaded successfully"
  else
   log_failure_msg "$PIDFILE does not exists"
  fi
  ;;
 *)
  # For invalid arguments, print the usage message.
  echo "Usage: $0 {start|stop|restart|reload|status}"
  exit 2
  ;;
esac
```

The above script basically provides a template for writing LSBInit scripts. You can change the DAEMON,PIDFILE,NAME variables, and the header to make this script, fit to your own programs.

To know more about the functions provided by the LSB, please refer to InitScript Functions

Once the Init script is done, in order to make the script to start or stop automatically, execute the following command. This will add appropriate ‘S’ and ‘K’ entries in the given run-levels. This will also add appropriate sequence number by considering the dependencies.

```sh
update-rc.d filename defaults
```