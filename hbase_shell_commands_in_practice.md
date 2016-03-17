# HBase Shell Commands in Practice


## HBase Shell Usage

* Quote all names in HBase Shell such as table and column names.
* Commas delimit command parameters.
* Type <RETURN> after entering a command to run it.
* Dictionaries of configuration used in the creation and alteration of tables are Ruby Hashes.
They look like this:

```hbase
{‘key1′ => ‘value1′, ‘key2′ => ‘value2′, …}
```

and are opened and closed with curley-braces.

* Key/values are delimited by the ‘=>’ character combination.
* Usually keys are predefined constants such as NAME, VERSIONS, COMPRESSION, etc.
* Constants do not need to be quoted. Type ‘Object.constants’ to see a (messy) list of all constants in the environment.
* If you are using binary keys or values and need to enter them in the shell, use double-quote’d hexadecimal representation. For example:

```hbase
hbase> get 't1', "keyx03x3fxcd"
hbase> get 't1', "key�03�23�11"
hbase> put 't1', "testxefxff", 'f1:', "x01x33x40"
```

The HBase shell is the (J)Ruby IRB with the below HBase-specific commands added.


## HBase Shell Commands

HBase Shell Commands can be categorized into below types.

* HBase Shell General Commands
* Data Definition Commands
* Data Manipulation Commands
* Other HBase Shell Commands


## General Commands

* status – shows the cluster status
* table_help – help on Table reference commands, scan, put, get, disable, drop etc.
* version – displays HBase version
* whoami – shows the current HBase user.

```hbase
hbase(main):001:0> help "status"
Command: status
Show cluster status. Can be 'summary', 'simple', or 'detailed'. The
default is 'summary'. Examples:

hbase> status
hbase> status 'simple'
hbase> status 'summary'
hbase> status 'detailed'
```


## DDL Commands


### alter

add/modify/delete column families, as well as change table configuration

**Add/Change column family**
For example, to change or add the ‘f1′ column family in table ‘t1′ from current value to keep a maximum of 5 cell VERSIONS, do:
```
hbase> alter 't1', NAME => 'f1', VERSIONS => 5
```
we can operate on several column families:
```
hbase> alter 't1', 'f1', {NAME => 'f2', IN_MEMORY => true}, {NAME => 'f3', VERSIONS => 5}
```

**Delete column family**
To delete the ‘f1′ column family in table ‘ns1:t1′, use one of:
```
hbase> alter 'ns1:t1', NAME => 'f1', METHOD => 'delete'
hbase> alter 'ns1:t1', 'delete' => 'f1'
```

**Alter Table Properties**
We can also change table-scope attributes like MAX_FILESIZE, READONLY, MEMSTORE_FLUSHSIZE, DEFERRED_LOG_FLUSH, etc. These can be put at the end;
for example, to change the max size of a region to 128MB, do:
```
hbase> alter 't1', MAX_FILESIZE => '134217728'
```

**alter_async**
Same as alter command but does not wait for all regions to receive the schema changes.

**alter_status**
Gets the status of the alter command. Indicates the number of regions of the table that have received the updated schema. Examples are
```
hbase> alter_status 't1'
hbase> alter_status 'ns1:t1'
```


## create

Used for Creating tables. Pass a table name, and a set of column family specifications (at least one), and, optionally, table configuration as arguments.

**Examples:**

1. Create a table with namespace=ns1 and table qualifier/name=t1
```
hbase> create 'ns1:t1', {NAME => 'f1', VERSIONS => 5}
```

2. Create a table with namespace=default and table qualifier=t1
```
hbase> create 't1', {NAME => 'f1'}, {NAME => 'f2'}, {NAME => 'f3'}
hbase> # The above in shorthand would be the following:
hbase> create 't1', 'f1', 'f2', 'f3'
hbase> create 't1', {NAME => 'f1', VERSIONS => 1, TTL => 2592000, BLOCKCACHE => true}
hbase> create 't1', {NAME => 'f1', CONFIGURATION => {'hbase.hstore.blockingStoreFiles' => '10'}}
```

3. Table configuration options can be put at the end.
```
hbase> create 'ns1:t1', 'f1', SPLITS => ['10', '20', '30', '40']
hbase> create 't1', 'f1', SPLITS => ['10', '20', '30', '40']
hbase> create 't1', 'f1', SPLITS_FILE => 'splits.txt', OWNER => 'johndoe'
hbase> create 't1', {NAME => 'f1', VERSIONS => 5}, METADATA => { 'mykey' => 'myvalue' }
hbase> # Optionally pre-split the table into NUMREGIONS, using
hbase> # SPLITALGO ("HexStringSplit", "UniformSplit" or classname)
hbase> create 't1', 'f1', {NUMREGIONS => 15, SPLITALGO => 'HexStringSplit'}
hbase> create 't1', 'f1', {NUMREGIONS => 15, SPLITALGO => 'HexStringSplit', CONFIGURATION => {'hbase.hregion.scan.loadColumnFamiliesOnDemand' => 'true'}}
```

4. We can also keep around a reference to the created table.
```
hbase> t1 = create 't1', 'f1'
```
Which gives a reference to the table named ‘t1′, on which we can then call methods t1.scan, t1.get.


## describe

Prints the schema of a table. We can also use abbreviated ‘desc’ for the same thing.
```
hbase(main):005:0> desc 'blog'
Table blog is ENABLED                                                                                                                           
COLUMN FAMILIES DESCRIPTION                                                                                                                     
{NAME => 'content', DATA_BLOCK_ENCODING => 'NONE', BLOOMFILTER => 'ROW', REPLICATION_SCOPE => '0', VERSIONS => '1', COMPRESSION => 'NONE', MIN_V
ERSIONS => '0', TTL => 'FOREVER', KEEP_DELETED_CELLS => 'FALSE', BLOCKSIZE => '65536', IN_MEMORY => 'false', BLOCKCACHE => 'true'}              
{NAME => 'info', DATA_BLOCK_ENCODING => 'NONE', BLOOMFILTER => 'ROW', REPLICATION_SCOPE => '0', VERSIONS => '1', COMPRESSION => 'NONE', MIN_VERS
IONS => '0', TTL => 'FOREVER', KEEP_DELETED_CELLS => 'FALSE', BLOCKSIZE => '65536', IN_MEMORY => 'false', BLOCKCACHE => 'true'}                 
2 row(s) in 0.1070 seconds

hbase(main):006:0>
```