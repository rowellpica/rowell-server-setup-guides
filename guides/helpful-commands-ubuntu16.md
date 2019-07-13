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
## Access a docker container with root privileged
```
docker exec -it --privileged --user root container_id /bin/bash
```
## tar/untar gzip a folder
```
tar czf name.tar.gz name/
tar -zxf name.tar.gz
```
## Count number of cores
```
# virtual cores
grep -c ^processor /proc/cpuinfo
# physical cores
grep ^cpu\\scores /proc/cpuinfo | uniq |  awk '{print $4}' 
```
## Check certificate validity
```
cat certificate.crt  | openssl x509 -noout -enddate
```