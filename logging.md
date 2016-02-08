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

```python
import logging
import sys

LEVELS = { 'debug':logging.DEBUG,
            'info':logging.INFO,
            'warning':logging.WARNING,
            'error':logging.ERROR,
            'critical':logging.CRITICAL,
            }

if len(sys.argv) > 1:
    level_name = sys.argv[1]
    level = LEVELS.get(level_name, logging.NOTSET)
    logging.basicConfig(level=level)

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical error message')
```

Run the script with an argument like ‘debug’ or ‘warning’ to see which messages show up at different levels:

```python
$ python logging_level_example.py debug

DEBUG:root:This is a debug message
INFO:root:This is an info message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical error message

$ python logging_level_example.py info

INFO:root:This is an info message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical error message
```

###Logging in Libraries

Developers of libraries, rather than applications, should also use logging. For them, there is even less work to do. Simply create a logger instance for each context, using an appropriate name, and then log messages using the stanard levels. As long as a library uses the logging API with consistent naming and level selections, the application can be configured to show or hide messages from the library, as desired.

###Naming Logger Instances

All of the previous log messages all have ‘root’ embedded in them. The logging module supports a hierarchy of loggers with different names. An easy way to tell where a specific log message comes from is to use a separate logger object for each module. Every new logger inherits the configuration of its parent, and log messages sent to a logger include the name of that logger. Optionally, each logger can be configured differently, so that messages from different modules are handled in different ways. Below is an example of how to log from different modules so it is easy to trace the source of the message:

```python
import logging

logging.basicConfig(level=logging.WARNING)

logger1 = logging.getLogger('package1.module1')
logger2 = logging.getLogger('package2.module2')

logger1.warning('This message comes from one module')
logger2.warning('And this message comes from another module')
```

And the output:
```python
$ python logging_modules_example.py

WARNING:package1.module1:This message comes from one module
WARNING:package2.module2:And this message comes from another module
There are many, many, more options for configuring logging, including different log message formatting options, having messages delivered to multiple destinations, and changing the configuration of a long-running application on the fly using a socket interface. All of these options are covered in depth in the library module documentation.
```