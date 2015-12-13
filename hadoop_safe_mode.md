# Hadoop Safe Mode

Safe mode maintenance command. Safe mode is a Namenode state in which it 

1. does not accept changes to the name space (read-only) 
2. does not replicate or delete blocks. 
Safe mode is entered automatically at Namenode startup, and leaves safe mode automatically when the configured minimum percentage of blocks satisfies the minimum replication condition. Safe mode can also be entered manually, but then it can only be turned off manually as well.