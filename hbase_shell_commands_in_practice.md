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