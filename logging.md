# Logging – Report status, error, and informational messages

The logging module defines a standard API for reporting errors and status information from applications and libraries. The key benefit of having the logging API provided by a standard library module is that all Python modules can participate in logging, so an application’s log can include messages from third-party modules.

###Logging in Applications

There are two perspectives for examining logging. Application developers set up the logging module, directing the messages to appropriate output channels. It is possible to log messages with different verbosity levels or to different destinations. Handlers for writing log messages to files, HTTP GET/POST locations, email via SMTP, generic sockets, or OS-specific logging mechanisms are all included, and it is possible to create custom log destination classes for special requirements not handled by any of the built-in classes.

###Logging to a File

Most applications are probably going to want to log to a file. Use the basicConfig() function to set up the default handler so that debug messages are written to a file.

```python
import logging

LOG_FILENAME = 'logging_example.out'
logging.basicConfig(filename=LOG_FILENAME,
                    level=logging.DEBUG,
                    )

logging.debug('This message should go to the log file')

f = open(LOG_FILENAME, 'rt')
try:
    body = f.read()
finally:
    f.close()

print 'FILE:'
print body
```

After running the script, the log message is written to logging_example.out:

```python
$ python logging_file_example.py

FILE:
DEBUG:root:This message should go to the log file
```

###Rotating Log Files

Running the script repeatedly causes more messages to be appended to the file. To create a new file each time the program runs, pass a filemode argument to basicConfig() with a value of 'w'. Rather than managing the creation of files this way, though, it is simpler to use a RotatingFileHandler: