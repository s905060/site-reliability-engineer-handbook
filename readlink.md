# Readlink

Read Value of Symbolic Link

```
readlink symbolic-link-name
```

```
-f, --canonicalize	canonicalize by following every symlink in every component of the given name recursively; all but the last component must exist.
-e, --canonicalize-existing	canonicalize by following every symlink in every component of the given name recursively, all components must exist
-m, --canonicalize-missing	canonicalize by following every symlink in every component of the given name recursively, without requirements on components existence
-n, --no-newline	do not output the trailing newline
-q, --quiet	suppress most error messages
-s, --silent	suppress error messages
-v, --verbose	report error messages
--help	display help and exit
--version	output version information and exit
```