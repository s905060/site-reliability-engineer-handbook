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