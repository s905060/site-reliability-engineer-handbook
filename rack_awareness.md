# Rack Awareness

HDFS, MapReduce, and YARN (Hadoop 2.0) are rack-aware components. This means they can learn about all the nodes of the cluster rack they belong to and act accordingly. The third block from default block placement policy (see the section “Block Placement and Replication in HDFS”) applies only if you have configured rack awareness.

If you have not configured rack awareness (in this case, all nodes of the cluster are considered to be on the same rack), or if you have a small cluster with just one rack, one replica of the block goes to the local data node and two replicas go to another two data nodes, selected randomly from the cluster.

>CAUTION
>
This should not be an issue if you have just one rack. However, a larger cluster with multiple racks faces the possibility that all replicas of a block end up in different data nodes in the same rack. This can cause data loss if that specific rack fails.

To avoid this problem, Hadoop (HDFS, MapReduce, and YARN) supports configuring rack awareness. This ensures that the third replica is written to a data node from another rack for better reliability and availability. Even if one rack is getting down, another rack then is available to serve the requests. This also increases the utilization of network bandwidth when reading data because data comes from multiple racks with multiple network switches.

###Making Clusters Rack Aware
You can make a Hadoop cluster rack aware by using a script that enables the master node to map the network topology of the cluster. It does so using the properties topology.script.file.name or net.topology.script.file.name, available in the core-site.xml configuration file.

First, you need to change this property to specify the name of the script file. Then you write the script and place it in a file at the specified location. The script should accept a list of IP addresses and return the corresponding list of rack identifiers. For example, the script takes host.foo.bar as an argument and returns /rack1 as the output.

In other words, the script should be able to accept IP addresses or DNS names and return the rack identifier; it is a one-to-one mapping between what the script takes and what it returns. For retrieving the rack identifier, the script might deduce it from the IP address or query some service (similar to the way DNS works). The simplest way is to read from a file that has the mapping from IP address or DNS name to rack identifier.

For example, imagine that you have a mapping file with the following information, where the first column represents the IP address or DNS name of the node and the second column represents the rack it belongs to:

```
hadoopdn001 /hadoop/rack1
hadoopdn002 /hadoop/rack1
hadoopdn006 /hadoop/rack2
hadoopdn007 /hadoop/rack2
```