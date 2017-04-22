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


references:

https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux
