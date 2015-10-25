# ltrace

It traces the calls to the library function. It executes the program in that process.
```
# ltrace /usr/bin/who
```

The output is shown below.
```
utmpxname(0x8050c6c, 0xb77068f8, 0, 0xbfc5cdc0, 0xbfc5cd78)          = 0
setutxent(0x8050c6c, 0xb77068f8, 0, 0xbfc5cdc0, 0xbfc5cd78)          = 1
getutxent(0x8050c6c, 0xb77068f8, 0, 0xbfc5cdc0, 0xbfc5cd78)          = 0x9ed5860
realloc(NULL, 384)                                                   = 0x09ed59e8
getutxent(0, 384, 0, 0xbfc5cdc0, 0xbfc5cd78)                         = 0x9ed5860
realloc(0x09ed59e8, 768)                                             = 0x09ed59e8
getutxent(0x9ed59e8, 768, 0, 0xbfc5cdc0, 0xbfc5cd78)                 = 0x9ed5860
realloc(0x09ed59e8, 1152)                                            = 0x09ed59e8
getutxent(0x9ed59e8, 1152, 0, 0xbfc5cdc0, 0xbfc5cd78)                = 0x9ed5860
realloc(0x09ed59e8, 1920)                                            = 0x09ed59e8
getutxent(0x9ed59e8, 1920, 0, 0xbfc5cdc0, 0xbfc5cd78)                = 0x9ed5860
getutxent(0x9ed59e8, 1920, 0, 0xbfc5cdc0, 0xbfc5cd78)                = 0x9ed5860
realloc(0x09ed59e8, 3072)                                            = 0x09ed59e8
getutxent(0x9ed59e8, 3072, 0, 0xbfc5cdc0, 0xbfc5cd78)                = 0x9ed5860
getutxent(0x9ed59e8, 3072, 0, 0xbfc5cdc0, 0xbfc5cd78)                = 0x9ed5860
getutxent(0x9ed59e8, 3072, 0, 0xbfc5cdc0, 0xbfc5cd78)
```

You can observe that there is a set of calls to getutxent and its family of library function. You can also note that ltrace gives the results in the order the functions are called in the program.

Now we know that ‘who’ command works by calling the getutxent and its family of function to get the logged in users.

