## 01 Create a SUDO user with Public Key Authentication
Change "sammy" to your preffered sudo user name
Execute from root
```
adduser sammy
usermod -aG sudo sammy
su - sammy
mkdir ~/.ssh && touch ~/.ssh/authorized_keys
chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

Execute from localmachine where you created your keys. This will copy your public key to the server's authorized_keys
```
scp ~/.ssh/your_key_rsa.pub sammy@SERVER_IP_ADDRESS:~/.ssh/authorized_keys
```

## 02 Disable Password Authentication
```
sudo vim /etc/ssh/sshd_config
```

Configurations that needs to be setup
```
  PasswordAuthentication no
  PubkeyAuthentication yes
  ChallengeResponseAuthentication no
```

Reload sshd service
```
sudo systemctl reload sshd
```

## 03 Install JDK8 (Optional)
```
sudo apt-get update
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo update-alternatives --config java # check the location of jdk
sudo vim /etc/environment
```
Add this line to set the JAVA_HOME in the environment variables
```
  JAVA_HOME="/usr/lib/jvm/java-8-oracle"
```
Load the new environment variable
```
source /etc/environment
```

## 04 Install MongoDB (Optional)
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
sudo apt-get update
sudo apt-get install mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
# sudo systemctl status mongod
```

Use Studio 3T to add an admin user. 
You can download it from [here](https://studio3t.com/).

Enable Authentication
```
sudo vim /etc/mongod.conf
```
```
security:
  authorization: "enabled"
```
Restart the mongod service
```
sudo systemctl restart mongod
```

## 05 Install Docker (Optional)
```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce
```
Install docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Executing the Docker Command Without Sudo (Optional)
```
sudo usermod -aG docker ${USER}
su - ${USER}
# id -nG
```

## 06 Install Redis (Optional)
```
sudo apt-get update
sudo apt-get install build-essential tcl
```
```
cd /tmp
curl -O http://download.redis.io/redis-stable.tar.gz
tar xzvf redis-stable.tar.gz
cd redis-stable
make
make test
sudo make install
```
Configure Redis
```
sudo mkdir /etc/redis
sudo cp /tmp/redis-stable/redis.conf /etc/redis
sudo vim /etc/redis/redis.conf
```
```
supervised systemd
dir /var/lib/redis
```
Create a Redis systemd Unit File
```
sudo vim /etc/systemd/system/redis.service
```
```
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```
Create the Redis User, Group and Directories
```
sudo adduser --system --group --no-create-home redis
sudo mkdir /var/lib/redis
sudo chown redis:redis /var/lib/redis
sudo chmod 770 /var/lib/redis
```
Start the Redis Service
```
sudo systemctl start redis
sudo systemctl enable redis
sudo systemctl status redis
```

## 07 Setup UFW (Uncomplicated Firewall)
```
sudo apt-get install ufw
```
```
sudo vim /etc/default/ufw`
>> # make sure IPV6 is set to 'yes'
>> IPV6=yes
```
```
# reset the default configuration
sudo ufw default deny incoming
sudo ufw default allow outgoing

# allow ssh (important)
sudo ufw allow ssh

# allow mysql db default port access if necessary
sudo ufw allow 3306

# allow redis-cli default port access if necessary
sudo ufw allow 6379

# enable ufw
sudo ufw enable

# check status
sudo ufw status
```