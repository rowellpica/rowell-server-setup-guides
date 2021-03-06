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
## Check linux folder size
```
$ du -h --max-depth=1 /
```
## Access a docker container with root privileged
```
$ docker exec -it --privileged --user root container_id /bin/bash
```
## tar/untar gzip a folder
```
$ tar czf name.tar.gz name/
$ tar -zxf name.tar.gz
```
## Count number of cores
```
# virtual cores
$ grep -c ^processor /proc/cpuinfo
# physical cores
$ grep ^cpu\\scores /proc/cpuinfo | uniq |  awk '{print $4}' 
```
## Check certificate validity
```
$ cat certificate.crt  | openssl x509 -noout -enddate
```
## Remove dangling docker images
```
$ docker rmi $(docker images -f "dangling=true" -q)
```
## Remove all exited docker containers
```
$ sudo docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm
```
## git commands
```
$ git diff --name-only
$ git checkout -- filename.txt
```
## autoscreencapture
```
$ while [ 1 ];do vardate=$(date +%d\-%m\-%Y\_%H.%M.%S); screencapture -t jpg -x ~/Desktop/project_name/$vardate.jpg; sleep 10; done
```
## node for sudo commands
```
n=$(which node); \
n=${n%/bin/node}; \
chmod -R 755 $n/bin/*; \
sudo cp -r $n/{bin,lib,share} /usr/local
```
## check port assignment
```
sudo lsof -i -P -n | grep LISTEN
```
## check for supported tls versions
```
sudo apt install nmap -y
nmap --script ssl-enum-ciphers -p 443 phstocks.pahina.xyz
```