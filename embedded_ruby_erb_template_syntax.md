# Embedded Ruby (ERB) Template Syntax

ERB is a templating language based on Ruby. Puppet can evaluate ERB templates with the template and inline_template functions.

This page covers how to write ERB templates. See Templates for information about what templates are and how to evaluate them.

Note: If you’ve used ERB in other projects, it might have had different features enabled. This page only describes how ERB works in Puppet.

###ERB Structure and Syntax
```
<%# Non-printing tag ↓ -%>
<% if @keys_enable -%>
<%# Expression-printing tag ↓ -%>
keys <%= @keys_file %>
<% unless @keys_trusted.empty? -%>
trustedkey <%= @keys_trusted.join(' ') %>
<% end -%>
<% if @keys_requestkey != '' -%>
requestkey <%= @keys_requestkey %>
<% end -%>
<% if @keys_controlkey != '' -%>
controlkey <%= @keys_controlkey %>
<% end -%>

<% end -%>
```

An ERB template looks like a plain-text document interspersed with tags containing Ruby code. When evaluated, this tagged code can modify text in the template.

Puppet passes data to templates via special objects and variables, which you can use in the tagged Ruby code to control the templates’ output.

###Tags
ERB has two tags for Ruby code, a tag for comments, and a way to escape tag delimiters.

* `<%= EXPRESSION %>` — Inserts the value of an expression.
 * With `-%>` — Trims the following line break.
* `<% CODE %>` — Executes code, but does not insert a value.
 * With `<%-` — Trims the preceding indentation.
 * With `-%>` — Trims the following line break.
* `<%# COMMENT %>` — Removed from the final output.
 * With `-%>` — Trims the following line break.
* `<%% or %%>` — A literal `<%` or `%>`, respectively.

Text outside a tag becomes literal text, but it is subject to any tagged Ruby code surrounding it. For example, text surrounded by a tagged if statement only appears in the output if the condition is true.

###Expression-Printing Tags

`<%= @fqdn %>`

An expression-printing tag inserts values into the output. It starts with an opening tag delimiter and equals sign (`<%=`) and ends with a closing tag delimiter (`%>`). It must contain a snippet of Ruby code that resolves to a value; if the value isn’t a string, it will be automatically converted to a string using its to_s method.

For example, to insert the value of the `$fqdn` and `$hostname` facts in an Apache config file, you could do something like:
```
ServerName <%= @fqdn %>
ServerAlias <%= @hostname %>
```

###Space Trimming
You can trim line breaks after expression-printing tags by adding a hyphen to the closing tag delimiter.

`-%>` — If the tag ends a line, trim the following line break.