# Puppet Module Cheat Sheet

###manifests
This directory holds the module's Puppet code.
* Each .pp file should contain one and only one
class or defined type.
* Filenames and class/defined type names are
related; see the examples below.
* Within a module, the special $module_name
variable always contains the module's name.


```
apache/manifests/init.pp
class apache {
...
}
init.pp​is special; it should contain a class
(or define) with the same name as the module.
apache/manifests/vhost.pp
define apache::vhost
($port, $docroot)
{
...
}
Other classes (and defines) should be named
modulename::filename​(without the .pp extension).
apache/manifests/config/ssl.pp
class apache::config::ssl {
...
}
Subdirectories add intermediate namespaces
```

