# HBase Shell Commands in Practice

**HBase Shell Usage**
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