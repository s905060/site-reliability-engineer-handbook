# tree


## View Different Directory Structures

* \# **tree / | more** – Execute this command to browse through the directory structure of the whole OS. Typically it doesn’t make sense to do tree on the root directory structure, unless you are in a learning mode and would like to understand the directory hierarchy of the Linux.
* \# **tree $HOME** – Use this to cross-verify the home directory structure content.
* \# **tree PROJECT-DIR** – Checking the directory structure of a project.

## Changing the output of the tree command.

* \# **tree -d** will display only the directories. i.e Files will not be displayed.
* \# **tree -a** will display hidden files along with directories and files.
* \# **tree -s** will display the file size as shown below. While using this option, it prints out the size of the files along with the file names.

```bash
# tree -s
.
|-- [       4096]  Articles
|   `-- [       4096]  Tree
|       `-- [       5489]  article
|-- [       4096]  Compression
|   |-- [       2584]  article
|   `-- [       4223]  article.safe
`-- [       4096]  DiskSpace
|-- [        722]  article
`-- [        530]  old_article
4 directories, 5 files
```

* \# tree -p will display the permissions along with the files. While using this option, it prints out the permissions of the files along with the file names as shown below.

```bash
# tree -p
.
|-- [drwx------]  Articles
|   `-- [drwx------]  Tree
|       `-- [-rw-------]  article
|-- [drwx------]  Compression
|   |-- [-rw-------]  article
|   `-- [-rw-------]  article.safe
`-- [drwx------]  DiskSpace
|-- [-rw-------]  article
`-- [-rw-------]  old_article

4 directories, 5 files
```

## Generate HTML output from tree command

You can also redirect the output of the tree command to a html file as shown below using the -H and -o option.

```bash
# tree -H . -o output.html
```

Definition of Option -H from the man page:

* **-H baseHREF**: Turn on HTML output, including HTTP references. Useful for ftp sites. baseHREF gives the base ftp location when using HTML output. That is, the local directory may be `/local/ftp/pub’, but it must be referenced as `ftp://hostname.organization.domain/pub’

## Display tree output based on the specified pattern

List the files that match the pattern using option -P as shown below.

```bash
Syntax: tree -P PATTERN
```

```bash
$ tree -P *.safe
.
|-- Articles
|   `-- Tree
|-- Compression
|   `-- article.safe
`-- DiskSpace
```

List the files that does not match the pattern using option -I as shown below.

```bash
Syntax: tree -I PATTERN
```

```bash
$ tree -I *.safe
.
|-- Articles
|   `-- Tree
|       `-- article
|-- Compression
|   `-- article
|-- DiskSpace
|   |-- article
|   `-- old_article
`-- t.html
```