# NGINX

## Installing with a package manager
```bash
$ sudo apt-get update
$ sudo apt-get install nginx
```

```bash
$ sudo ps aux | grep nginx
```
```bash
root      3448  0.0  0.0 141108  1544 ?        Ss   22:36   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data  3450  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
www-data  3452  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
www-data  3454  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
www-data  3456  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
www-data  3458  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
www-data  3459  0.0  0.0 143784  6216 ?        S    22:36   0:00 nginx: worker process
ksc       4391  0.0  0.0  22820  1020 pts/2    S+   22:40   0:00 grep --color=auto nginx
```
```bash
$ ifconfig
```
- Type localhost on browser

## Adding an NGINX Service

- Check options for nginx
```bash
$ nginx -h

ksc@ksc-ubuntu:~$ nginx -h
nginx version: nginx/1.14.0 (Ubuntu)
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/share/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```
- Stop nginx
```bash
$ nginx -s stop
$ sudo ps aux | grep nginx
```
- Create service file
```bash
$ touch /lib/systemd/system/nginx.service
# https://www.nginx.com/resources/wiki/start/topics/examples/systemd/
$ vim /lib/systemd/system/nginx.service

[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
```bash
$ systemctl start nginx
$ sudo ps aux | grep nginx
$ systemctl status nginx
$ systemctl stop nginx
# check browser
$ systemctl enable nginx
```
```bash
$ reboot
```

```bash
$ systemctl status nginx
# check browser
```
