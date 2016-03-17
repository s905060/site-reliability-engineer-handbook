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