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

```python
import glob
import logging
import logging.handlers

LOG_FILENAME = 'logging_rotatingfile_example.out'

# Set up a specific logger with our desired output level
my_logger = logging.getLogger('MyLogger')
my_logger.setLevel(logging.DEBUG)

# Add the log message handler to the logger
handler = logging.handlers.RotatingFileHandler(LOG_FILENAME,
                                               maxBytes=20,
                                               backupCount=5,
                                               )
my_logger.addHandler(handler)

# Log some messages
for i in range(20):
    my_logger.debug('i = %d' % i)

# See what files are created
logfiles = glob.glob('%s*' % LOG_FILENAME)
for filename in logfiles:
    print filename
```

The result should be six separate files, each with part of the log history for the application:
```python
$ python logging_rotatingfile_example.py

logging_rotatingfile_example.out
logging_rotatingfile_example.out.1
logging_rotatingfile_example.out.2
logging_rotatingfile_example.out.3
logging_rotatingfile_example.out.4
logging_rotatingfile_example.out.5
```

The most current file is always logging_rotatingfile_example.out, and each time it reaches the size limit it is renamed with the suffix .1. Each of the existing backup files is renamed to increment the suffix (.1 becomes .2, etc.) and the .5 file is erased.

>Note Obviously this example sets the log length much much too small as an extreme example. Set maxBytes to a more appropriate value in a real program.

###Verbosity Levels

Another useful feature of the logging API is the ability to produce different messages at different log levels. This code to be instrumented with debug messages, for example, while setting the log level down so that those debug messages are not written on a production system.

|Level	|Value
|--|--
|CRITICAL|	50
|ERROR|	40
|WARNING|	30
|INFO|	20
|DEBUG|	10
|UNSET|	0

The logger, handler, and log message call each specify a level. The log message is only emitted if the handler and logger are configured to emit messages of that level or higher. For example, if a message is CRITICAL, and the logger is set to ERROR, the message is emitted (50 > 40). If a message is a WARNING, and the logger is set to produce only messages set to ERROR, the message is not emitted (30 < 40).