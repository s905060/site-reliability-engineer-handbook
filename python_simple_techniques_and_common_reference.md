# Python simple techniques and common reference

###python file support Chinese
```
# -*- coding: UTF-8 -*-
```

###Execute shell commands
```
from subprocess import Popen, PIPE
def run_cmd(cmd):
    #Popen call wrapper.return (code, stdout, stderr)
    child = Popen(cmd, stdin = PIPE, stdout = PIPE, stderr = PIPE, shell = True)
    out, err = child.communicate()
    ret = child.wait()
    return (ret, out, err)
```

###Get the current path python script file
```
import os
os.path.split(os.path.realpath(__file__))[0]
```

###json module import problems
```
try :
    import json
except :
    import simplejson as json
```

###Json json format using tools
```
#Below python 2.7
echo '{"hello":1}' | python -m simplejson.tool
#Above python 2.7
echo '{"hello":1}' | python -m json.tool
```

###Get URL resources

```
import urllib2
response = urllib2.urlopen('http://www.opstool.com/')
html = response.read()
print html
```

###About main
```
if __name__=='__main__':
```

###Date specified format
```
import datetime
yesterday =datetime.date.today() -datetime.timedelta(days=1)
pt = yesterday.strftime("%Y%m%d")
print pt
```

###timestamp into date and time
```
timeStamp = 1437122504
timeArray = time.localtime(timeStamp)
otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
```

##Datetime string into timestamp
```
int(time.mktime( time.strptime("2015-07-21 10:23:00", "%Y-%m-%d %H:%M:%S")))
```

###Get the current time
```
import datetime
datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

###Use option
```
import getopt
try:
    opts, args = getopt.getopt(sys.argv[1:], "t:")
    for op, value in opts:
        if op == '-t':
            gctime =  int(value)
except getopt.GetoptError:
    sys.exit(1)
```

Use log
```
import os
import logging

def get_logger(logger_name):
    # Create logger
    logger = logging.getLogger(logger_name)
    logger.setLevel(logging.DEBUG)

    # Create a handler, written to a log file for 
    FH = logging . FileHandler ( OS . path . split ( OS . path . realpath ( __file__ )) [ 0 ] + "/" + logger_name + ".log" ) 
    FH . setLevel ( logging . DEBUG )

    # Then create a handler, for output to the console 
    ch = logging . StreamHandler () 
    ch . setLevel ( logging . DEBUG )

    # Define handler's output format
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    fh.setFormatter(formatter)
    ch.setFormatter(formatter)

    # Add logger handler
    logger.addHandler(fh)
    logger.addHandler(ch)
    return logger

logger=get_logger("mylogger")
```

### Achieve deamon run

```
import sys
import us

def daemonize (stdin='/dev/null', stdout='/dev/null', stderr='/dev/null'):
    # Perform first fork.
    try:
        pid = os.fork( )
        if pid > 0:
            sys.exit(0) # Exit first parent.
    except OSError, e:
        sys.stderr.write("fork #1 failed: (%d) %sn" % (e.errno, e.strerror))
        sys.exit(1)
    # Decouple from parent environment.
    os.chdir("/")
    os.umask(0)
    os.setsid( )
    # Perform second fork.
    try:
        pid = os.fork( )
        if pid > 0:
            sys.exit(0) # Exit second parent.
    except OSError, e:
        sys.stderr.write("fork #2 failed: (%d) %sn" % (e.errno, e.strerror))
        sys.exit(1)
    # The process is now daemonized, redirect standard file descriptors.
    for f in sys.stdout, sys.stderr: f.flush( )
    si = file(stdin, 'r')
    so = file(stdout, 'a+')
    se = file(stderr, 'a+', 0)
    os.dup2(si.fileno( ), sys.stdin.fileno( ))
    os.dup2(so.fileno( ), sys.stdout.fileno( ))
    os.dup2(se.fileno( ), sys.stderr.fileno( ))
    
    #Into the background 
daemonize ()
```

###Simple HTTP server example

```
import urllib
from cgi import parse_header, parse_multipart
from urlparse import parse_qs

from BaseHTTPServer import HTTPServer, BaseHTTPRequestHandler
class TestHTTPHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        buf="Hello World!"
        self.protocal_version = "HTTP/1.1"
        self.send_response(200)
        self.end_headers()
        self.wfile.write(buf)

    def do_POST(self):
        self.protocal_version = "HTTP/1.1"
        self.send_response(200)
        self.end_headers()
        buf = self._parse_POST()
        self.wfile.write(buf['postvar'])

    def _parse_POST(self):
        ctype, pdict = parse_header(self.headers['content-type'])
        if ctype == 'multipart/form-data':
            postvars = parse_multipart(self.rfile, pdict)
        elif ctype == 'application/x-www-form-urlencoded':
            length = int(self.headers['content-length'])
            postvars = parse_qs(
                    self.rfile.read(length),
                    keep_blank_values=1)
        else:
            postvars = {}
        return postvars
def start_server():
    http_server = HTTPServer(("127.0.0.1", 8088), TestHTTPHandler)
    http_server.serve_forever()

start_server()
```

###A functional method call

```
#!/bin/env python

def mytest():
    print "hello"

m={
    'myfunction' : 'mytest'
}

Global () [m ['MyFunction']] ()

print Global ()
```
