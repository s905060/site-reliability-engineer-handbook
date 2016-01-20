# VIM

vi/vim editor FAQ: Can you share some example vi/vim delete commands?

The vi editor can be just a little difficult to get started with, so I thought I’d share some more vi commands here today, specifically some commands about how to delete text in vi/vim.

###vi/vim delete commands - reference
A lot of times all people need is a quick reference, so I’ll start with a quick reference of vi/vim delete commands:

```
x   - delete current character
dw  - delete current word
dd  - delete current line
5dd - delete five lines

d$  - delete to end of line
d0  - delete to beginning of line

:1,.d
delete to beginning of file

:.,$d
delete to end of file
```