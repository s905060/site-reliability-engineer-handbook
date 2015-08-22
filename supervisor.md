# Supervisor

Example:

supervisord.conf

```
[supervisord]
;logfile=/var/log/supervisor/supervisord-nobody.log  ; (main log file;default $CWD/supervisord.log)
;logfile_maxbytes=50MB       ; (max main logfile bytes b4 rotation;default 50MB)
;logfile_backups=10          ; (num of main logfile rotation backups;default 10)
;loglevel=info               ; (log level;default info; others: debug,warn,trace)
;pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
nodaemon=true                ; (start in foreground if true;default false)
;user=nobody

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[program:php5-fpm]
command=/usr/sbin/php-fpm -c /etc/php-fpm.d
numprocs=1
autostart=true
autorestart=true

[program:php5-fpm-log]
command=tail -f /var/log/php5-fpm.log
stdout_events_enabled=true
stderr_events_enabled=true

[program:nginx]
command=/usr/sbin/nginx
numprocs=1
autostart=true
autorestart=true

```