### Author rowellpica

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
  JAVA_HOME="/usr/lib/jvm/java-8-oracle"
source /etc/environment
```