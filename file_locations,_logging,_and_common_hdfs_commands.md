# File Locations, Logging, and Common HDFS Commands

##File Locations

Configuration files: These files are used to configure a hadoop cluster.

* 
**core-site.xml:** All Hadoop services and clients use this file to locate the NameNode, so this file must be copied to each node that is either running a Hadoop service or is a client node. The Secondary NameNode uses this file to determine the location for storing fsimage and edits log namefs.checkpoint.dir/name locally, and the location of the NameNode namefs.namedefault.name/name.

Use the **core-site.xml** file to isolate communication issues with the NameNode host machine.

* 
**hdfs-site.xml:** HDFS services use this file, which contains a number of important properties. These include:

  * HTTP addresses for the two services

  * Replication for DataNodes namedfs.replication/name>

  * DataNode block storage location namedfs.data.dir/name

  * NameNode metadata storage namedfs.name.dir/name

Use the hdfs-site.xml file to isolate NameNode start-up issues. Typically, NameNode start-up issues are caused when NameNode fails to load the fsimage and edits log to merge. Ensure that the values for the location properties in hdfs-site.xml are valid locations.

**datanode.xml:**

DataNode services use the datanode.xml file to specify the maximum and minimum heap size for the DataNode service. To troubleshoot issues with DataNode: change the value for -Xmx, which changes the maximum heap size for the DataNode service. Restart the affected DataNode host machine.

**namenode.xml:**

NameNode services use the namenode.xml file to specify the maximum and minimum heap size for the NameNode service. To troubleshoot issues with NameNode, change the value for -Xmx, which changes the maximum heap size for NameNode service. Restart the affected NameNode host machine.

**secondarynamenode.xml:**

Secondary NameNode services use the secondarynamenode.xml file to specify the maximum and minimum heap size for the Secondary NameNode service. To troubleshoot issues with Secondary NameNode, change the value for -Xmx, which changes the maximum heap size for Secondary NameNode service. Restart the affected Secondary NameNode host machine.

**hadoop-policy.xml:**

Use the hadoop-policy.xml file to configure service-level authorization/ACLs within Hadoop. NameNode accesses this file. Use this file to troubleshoot permission related issues for NameNode.

**log4j.properties:**

Use the log4j.properties file to modify the log purging intervals of the HDFS logs. This file defines logging for all the Hadoop services. It includes, information related to appenders used for logging and layout. For more details, see the log4j documentation.

Log Files: Following are sets of log files for each of the HDFS services. They are stored in c:\hadoop\logs\hadoop and c:\hdp\hadoop-1.1.0- SNAPSHOT\bin by default.

HDFS .out files: Log files with the .out extension are located in c:\hdp\hadoop-1.1.0-SNAPSHOT\bin. They have the following naming conventions:

**datanode.out.log**

**namenode.out.log**

**secondarynamenode.out.log**

These files are created and written to when HDFS services are bootstrapped. Use these files to isolate launch issues with DataNode, NameNode, or Secondary NameNode services.

HDFS .wrapper files: The log files with the .wrapper extension are located in c:\hdp\hadoop-1.1.0-SNAPSHOT\bin and have the following file names:

**datanode.wrapper.log**

**namenode.wrapper.log**
**secondarynamenode.wrapper.log**

These files contain the start-up command string to start the service, and list process ID output on service start-up.

HDFS .log and .err files:

The following files are located in c:\hdp\hadoop-1.1.0-SNAPSHOT\bin:

**datanode.err.log**

**namenode.err.log**

**secondarynamenode.err.log**

The following files are located in c:\hadoop\logs\hadoop:

**hadoop-datanode-Hostname.log**

**hadoop-namenode-Hostname.log**

**hadoop-secondarynamenode-Hostname.log**

These files contain log messages for the running Java service. If there are any errors encountered while the service is already running, the stack trace of the error is logged in the above files.

Hostname is the host where the service is running. For example, on a node where the host name is host3, the file would be saved as hadoop-namenode-host3.log.

[Note]	Note
By default, these log files are rotated daily. Use the c:\hdp\hadoop-1.1.0- SNAPSHOT\conf\log4j.properties file to change log rotation frequency.

HDFS <.date> files:

Log files with the <.date> extension have the following format:

**hadoop-namenode-$Hostname.log.<date>**

**hadoop-datanode-$Hostname.log.<date>**

**hadoop-secondarynamenode-$Hostname.log.<date>**

When a .log file is rotated, the current date is appended to the file name; for example: hadoop-datanode- hdp121.localdomain.com.log.2013-02-08.

Use these files to compare the past state of your cluster with the current state, to identify potential patterns.