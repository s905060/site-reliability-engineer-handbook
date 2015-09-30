# Logrotate

```
# sample logrotate configuration file
       compress

       /var/log/messages {
           rotate 5
           weekly
           postrotate
                                     /sbin/killall -HUP syslogd
           endscript
       }

       "/var/log/httpd/access.log" /var/log/httpd/error.log {
           rotate 5
           mail www@my.org
           size=100k
           sharedscripts
           postrotate
                                     /sbin/killall -HUP httpd
           endscript
       }

       /var/log/news/news.crit {
           monthly
           rotate 2
           olddir /var/log/news/old
           missingok
           postrotate
                                     kill -HUP ‘cat /var/run/inn.pid‘
           endscript
           nocompress
       }
```

```
Here is more information on the directives which may be included  in  a
       logrotate configuration file:


       compress
              Old  versions  of log files are compressed with gzip by default.
              See also nocompress.


       compresscmd
              Specifies which command to  use  to  compress  log  files.   The
              default is gzip.  See also compress.


       uncompresscmd
              Specifies  which  command  to  use to uncompress log files.  The
              default is gunzip.


       compressext
              Specifies which extension to use on compressed logfiles, if com-
              pression is enabled.  The default follows that of the configured
              compression command.


       compressoptions
              Command line options may be passed to the  compression  program,
              if  one is in use.  The default, for gzip, is "-9" (maximum com-
              pression).


       copy   Make a copy of the log file, but don’t change  the  original  at
              all.   This option can be used, for instance, to make a snapshot
              of the current log file, or when some  other  utility  needs  to
              truncate or pare the file.  When this option is used, the create
              option will have no effect, as the old log file stays in  place.


       copytruncate
              Truncate  the  original log file in place after creating a copy,
              instead of moving the old log file and optionally creating a new
              one,  It  can be used when some program can not be told to close
              its logfile and thus might continue writing (appending)  to  the
              previous log file forever.  Note that there is a very small time
              slice between copying the file and truncating it, so  some  log-
              ging  data  might be lost.  When this option is used, the create
              option will have no effect, as the old log file stays in  place.


       create mode owner group
              Immediately after rotation (before the postrotate script is run)
              the log file is created (with the same name as the log file just
              rotated).   mode  specifies  the  mode for the log file in octal
              (the same as chmod(2)), owner specifies the user name  who  will
              own  the  log  file,  and group specifies the group the log file
              will belong to. Any of the log file attributes may  be  omitted,
              in  which  case  those  attributes for the new file will use the
              same values as the original log file for the omitted attributes.
              This option can be disabled using the nocreate option.


       daily  Log files are rotated every day.


       delaycompress
              Postpone  compression of the previous log file to the next rota-
              tion cycle.  This has only effect when used in combination  with
              compress.   It  can be used when some program can not be told to
              close its logfile and thus might continue writing to the  previ-
              ous log file for some time.


       extension ext
              Log  files  are given the final extension ext after rotation. If
              compression is used, the compression  extension  (normally  .gz)
              appears after ext.


       ifempty
              Rotate  the  log  file  even  if  it  is  empty,  overiding  the
              notifempty option (ifempty is the default).


       include file_or_directory
              Reads the file given as an argument as if it was included inline
              where  the  include  directive appears. If a directory is given,
              most of the files in that directory are read in alphabetic order
              before  processing  of  the  including  file continues. The only
              files which are ignored are files which are  not  regular  files
              (such  as directories and named pipes) and files whose names end
              with one of the taboo extensions, as specified by  the  tabooext
              directive.  The include directive may not appear inside of a log
              file definition.


       mail address
              When a log is rotated out-of-existence, it is mailed to address.
              If  no  mail should be generated by a particular log, the nomail
              directive may be used.


       mailfirst
              When using the mail command, mail the just-rotated file, instead
              of the about-to-expire file.


       maillast
              When  using  the  mail  command,  mail the about-to-expire file,
              instead of the just-rotated file (this is the default).


       missingok
              If the log file is missing, go on to the next one without  issu-
              ing an error message. See also nomissingok.


       monthly
              Log files are rotated the first time logrotate is run in a month
              (this is normally on the first day of the month).


       nocompress
              Old versions of log files are not compressed with gzip. See also
              compress.


       nocopy Do  not copy the original log file and leave it in place.  (this
              overrides the copy option).


       nocopytruncate
              Do not truncate the original log file in place after creating  a
              copy (this overrides the copytruncate option).


       nocreate
              New  log  files  are  not  created  (this  overrides  the create
              option).


       nodelaycompress
              Do not postpone compression of the previous log file to the next
              rotation cycle (this overrides the delaycompress option).


       nomail Don’t mail old log files to any address.


       nomissingok
              If  a  log  file  does  not  exist,  issue an error. This is the
              default.


       noolddir
              Logs are rotated in the same directory the log normally  resides
              in (this overrides the olddir option).


       nosharedscripts
              Run  prerotate  and postrotate scripts for every script which is
              rotated (this is the default, and  overrides  the  sharedscripts
              option).


       notifempty
              Do not rotate the log if it is empty (this overrides the ifempty
              option).


       olddir directory
              Logs are moved into directory for rotation. The  directory  must
              be  on  the  same physical device as the log file being rotated,
              and is assumed to be relative to the directory holding  the  log
              file unless an absolute path name is specified. When this option
              is used all old versions of the log end up in  directory.   This
              option may be overriden by the noolddir option.


       postrotate/endscript
              The  lines  between postrotate and endscript (both of which must
              appear on lines by themselves) are executed after the  log  file
              is  rotated.  These  directives  may only appear inside of a log
              file definition.  See prerotate as well.


       prerotate/endscript
              The lines between prerotate and endscript (both  of  which  must
              appear  on lines by themselves) are executed before the log file
              is rotated and only if the log will actually be  rotated.  These
              directives may only appear inside of a log file definition.  See
              postrotate as well.


       firstaction/endscript
              The lines between firstaction and endscript (both of which  must
              appear  on lines by themselves) are executed once before all log
              files that match the wildcarded pattern are rotated, before pre-
              rotate  script is run and only if at least one log will actually
              be rotated. These directives may only appear  inside  of  a  log
              file definition. See lastaction as well.


       lastaction/endscript
              The  lines  between lastaction and endscript (both of which must
              appear on lines by themselves) are executed once after  all  log
              files  that  match  the  wildcarded  pattern  are rotated, after
              postrotate script is run  and  only  if  at  least  one  log  is
              rotated.  These  directives may only appear inside of a log file
              definition. See lastaction as well.


       rotate count
              Log files are rotated <count>  times  before  being  removed  or
              mailed to the address specified in a mail directive. If count is
              0, old versions are removed rather then rotated.


       size size
              Log files are rotated when they grow bigger then size bytes.  If
              size  is  followed by M, the size if assumed to be in megabytes.
              If the k is used, the size is in kilobytes. So  size  100,  size
              100k, and size 100M are all valid.


       sharedscripts
              Normally,  prescript and postscript scripts are run for each log
              which is rotated, meaning that a single script may be run multi-
              ple  times for log file entries which match multiple files (such
              as the /var/log/news/* example). If sharedscript  is  specified,
              the scripts are only run once, no matter how many logs match the
              wildcarded pattern.  However, if none of the logs in the pattern
              require  rotating,  the  scripts  will  not  be run at all. This
              option overrides the nosharedscripts option and  implies  create
              option.


       start count
              This is the number to use as the base for rotation. For example,
              if you specify 0, the logs will be created with a  .0  extension
              as they are rotated from the original log files.  If you specify
              9, log files will be created with a  .9,  skipping  0-8.   Files
              will  still  be  rotated  the number of times specified with the
              count directive.


       tabooext [+] list
              The current taboo extension list is  changed  (see  the  include
              directive  for information on the taboo extensions). If a + pre-
              cedes the list of extensions, the current taboo  extension  list
              is  augmented,  otherwise  it is replaced. At startup, the taboo
              extension list contains .rpmorig, .rpmsave, ,v,  .swp,  .rpmnew,
              and ~.


       weekly Log  files  are  rotated if the current weekday is less then the
              weekday of the last rotation or if more then a week  has  passed
              since  the  last rotation. This is normally the same as rotating
              logs on the first day of the week, but it works better if logro-
              tate is not run every night.
```