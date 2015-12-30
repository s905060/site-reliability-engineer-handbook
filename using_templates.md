# Using Templates

Templates are documents that combine code, data, and literal text to produce a final rendered output. The goal of a template is to manage a complicated piece of text with simple inputs.

In Puppet, you’ll usually use templates to manage the content of configuration files (via the content attribute of the file resource type).

###Templating Languages
Templates are written in a templating language, which is specialized for generating text from data.

Puppet supports two templating languages:

* Embedded Puppet (EPP) uses Puppet expressions in special tags. It’s easy for any Puppet user to read, but only works with newer Puppet versions. (≥ 4.0, or late 3.x versions with future parser enabled.)
* Embedded Ruby (ERB) uses Ruby code in tags. You need to know a small bit of Ruby to read it, but it works with all Puppet versions.