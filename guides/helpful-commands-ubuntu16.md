## View recent ssh sessions
```
$ last
```
## View authentication related logs
```
$ sudo less /var/log/auth.log
```
## View and Delete files older than 5 days
```
$ find /path/to/files -mindepth 1 -mtime +5 -print
$ find /path/to/files -mindepth 1 -mtime +5 -delete
```