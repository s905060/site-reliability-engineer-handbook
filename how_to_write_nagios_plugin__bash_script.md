# How To Write Nagios Plugin – Bash Script

Nagios is IT Infrastructure Monitoring Tool. Nagios is designed as a client/server model application and therefore needs two components to work – Nagios server and Nagios client or agent (NRPE for linux or NSClient++ for Windows). Nagios server is the one who connects to Nagios client and triggers a Nagios plugin to get information from Nagios client. Nagios plugin can be any script which can execute on the Nagios client machine (usually perl or bash script). Of course Nagios plugin scripts must be written to output the correct responses and exit codes Nagios server can understand. In this post we will learn how to write bash Nagios plugin script with correct response and exit codes.

###Let’s write a Nagios Plugin Bash Script!
Prerequisites:
* Working Nagios Server Instance (Read more here for Ubuntu or CentOS)
* Working Nagios NRPE Client Instance

###Important:
* Exit Code 0 = OK
If Nagios plugin script is successfully executed and exits with an exit code 0 then Nagios server will show this check as OK and color it with GREEN color.

* Exit Code 1 = WARNING
If Nagios plugin script is successfully executed and exits with an exit code 1 then Nagios server will show this check as a WARNING and color it with ORANGE color.

* Exit Code 2 = CRITICAL
If Nagios plugin script is successfully executed and exits with an exit code 2 then Nagios server will show this check as CRITICAL and color it with RED color.

###Location of Nagios Plugin Scripts:
* If you compiled your NRPE Client yourself the location of Nagios Plugins will probably be:/usr/local/nagios/libexec
* If you installed NRPE Client with YUM or from RPM the location of Nagios Plugins will be:/usr/lib/nagios/plugins
* If you installed NRPE Client with apt-get or from DEB the location of Nagios Plugins will also be:/usr/lib/nagios/plugins
If you agree, it is only fair we put our own Nagios Plugin Scripts in the same directory as other Nagios Plugin Scripts are.

###Working Example:
For a working example i have written a simple bash script which checks the number of currently opened files for the specified user with the specified WARNING and CRITICAL threshold. Make sure you will make your Nagios Plugin Bash script executable before you run it! Please note the bolded EXIT CODES in my bash script! You must have lsof installed on your Nagios Client for this script to work (yum install lsof).