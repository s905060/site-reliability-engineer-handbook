# hadoop Management Command --dfsadmin

dfsadmin is a multitasking tool that we can use it to get HDFS status information, and a series of management actions on HDFS execution.

###Called
     example: `hadoop dfsadmin -report`
     
###dfsadmin command Detailed 

`-report` : Check the file system's basic information and statistics.

`-safeadmin enter | leave | get | wait` : Safe Mode command. Safe Mode is a state NameNode, in this state, NameNode not accept changes to the name space (read-only); not copy or delete blocks. NameNode automatically at boot into safe mode, when the minimum percentage of the configuration block satisfying the condition of the minimum number of copies, it will automatically leave safe mode. enter is to enter, leave leave.

`-refreshNodes` : re-read the hosts and exclude files, so that the new node or nodes in the cluster need to exit NameNode can be re-identified. This command is used when adding a new node or log node.

`-finalizeUpgrade` : End of HDFS upgrade operations. DataNode delete the previous version of the working directory, after NameNode do likewise.

`-upgradeProgress status | details | Force` : Request the current state of the system upgrade | Details upgrade status | mandatory upgrade operation

`-metasave filename` : <filename> file directory where you saved NameNode main data structure to hadoop.log.dir property designated under the.

`-setQuota <quota> <dirname> ...... <dirname>` : set quota <quota> for each directory <dirname>. Directory quota is a long integer integer, force-set number of names in the directory tree.

`-clrQuota <dirname> ...... <dirname>` : for each directory <dirname> Clear quota setting.

`-help`  : This command will not say more.