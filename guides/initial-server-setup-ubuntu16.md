## Create a SUDO user with Public Key Authentication
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

## Disable Password Authentication

Execute from new sudo user (e.g. sammy)
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

## Install JDK8 (Optional)
Execute from new sudo user (e.g. sammy)
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

## Install MongoDB
Execute from new sudo user
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

## Install Docker
Execute from new sudo user
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