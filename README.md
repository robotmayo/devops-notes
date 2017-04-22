# devops-notes

### On a fresh server

Create a new user
```bash
adduser <name>
```

Put them in sudoers:
```bash 
gpasswd -a <name> sudo
```

Locally, generate a new keypair
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
eval "$(ssh-agent -s)" # start ssh agent
ssh-add ~/.ssh/<private key file>
```

Add the generated public key to authorized_keys
```bash
su - demo # Assuming you are still logged in as root
mkdir .ssh
chmod 700 .ssh
vim .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

Disable root login
```bash
vim /etc/ssh/sshd_config
#find the PermitRootLogin 
PermitRootLogin no
```

Restart ssh
```bash
service ssh restart
```

## UFW(Firewall) Basic Setup

Allow ssh
```bash 
sudo ufw allow ssh
```
If its on a different port

```bash 
sudo ufw allow 4444/tcp
```

Enable IPv6
```bash
sudo vim /etc/default/ufw
```
Find and change to `IPV6=yes`

Stats
```bash 
sudo ufw status verbose
```

UFW Defaults
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

UFW has some default aliases
```bash
sudo ufw allow http
sudo ufw allow https
sudo ufw allow ftp
```

Port Ranges
```bash
sudo ufw allow 6000:6007/udp
sudo ufw allow 6000:6007/tcp
```

Spefic ips
```bash
sudo ufw allow from 15.15.15.51
sudo ufw allow from 15.15.15.51 to any port 22
```

Subnets
```bash
sudo ufw allow from 15.15.15.0/24
sudo ufw allow from 15.15.15.0/24 to any port 22
```

Network interfaces
```bash
ip addr # list the network interfaces
# Eg
# 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state
sudo ufw allow in on eth0 to any port 80
sudo ufw allow in on eth1 to any port 3306
```

Denying
Just replace allow with deny lol

Deleting Rules
You can delete things by number
```bash
sudo ufw status numbered # list by number
sudo ufw delete 2
```

Or by rule
```bash
sudo ufw delete allow 80
```




references:

https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux
https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-14-04
