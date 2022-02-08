# pocketnotes

A.
apt update
apt upgrade

reboot 

B.
# creates a new user
#!/bin/bash
adduser <user>
usermod -aG sudo <user>
cp -r /root/.ssh /home/<user>/.ssh
chown -R <user>:<user> /home/<user>/.ssh
su - <user>

C. 
New dependencyinstaller.sh
#!/bin/bash

sudo apt-get update -y
sudo apt-get install golang -y
sudo apt install nginx -y
sudo apt install certbot python3-certbot-nginx -y
sudo apt install git -y
curl -sSL https://git.io/g-install | sh -s
echo STARTING NEW SHELL TO LOAD G-INSTALL...
sudo su - $(whoami)


D. SSL 

Set up an A Record of your subdomain on your DNS wrt the IP Address

# BenVan's version
sudo certbot certonly -d YourDomainName --manual --preferred-challenges dns

# changed to 
sudo certbot certonly -d YourDomainName --preferred-challenges http

It asks the standard questions but for verification 
select webroot then enter /var/www/html 

a. You no longer need a TXT record in your DNS. 
Instead, if I understand correctly, it puts a file in your home directory checks that https
is working then deletes temporary file.

B.2 Test out the renewal of the certbot

# check to see the certbot renewal crontab is working
sudo systemctl status certbot.timer

# check to see if it will renew the domain SSL as a Dry Run
sudo sudo certbot certonly --dry-run -d <yourdomain.com>


E. Change then run  Ben Vans install.sh script with these changes 

# change to 1.17
install 1.17
# replace this with next 4 lines "go get -u github.com/pokt-network/pocket-core"
cd go
mkdir -p src/github.com/pokt-network
cd src/github.com/pokt-network
git clone https://github.com/pokt-network/pocket-core.git

and 
echo "Updating Go to V 1.17"
g install 1.17

copy install.sh to the home directory of you user 

./install.sh

F.  BootStrap
A. Start a Screen Session

screen (then press space bar)

Go here to get the latest BootStrap

https://github.com/pokt-network/pocket-snapshots

Copy latest snapshot.

Many methods - this uses the first tar directly into the ../data folder

cd .pocket/data 

1.c Just one  node then cd .pocket/data (Don't copy this one - copy the latest form teh link above!)
wget -qO- https://link.us1.storjshare.io/raw/juqzyexh64g4iq7q36g24xyvbvfa/pocket-public-blockchains/pocket-network-data-0009-rc-0.6.3.6.tar | tar xvf -


To save time use Screen
Ctrl A Crtl D to leaver 
then 
screen -ls to see the screen number

to return 

screen -r <screen number>

H.  Add as boot service

sudo cd /etc/systemd/system
sudo vi poktnode.service

Add the following lines

[Unit]
Description=pocket service node
After=network-online.target
[Service]
Type=simple
User=<user>
ExecStart=/home/<user>/go/bin/pocket start
Restart=always
RestartSec=30s
[Install]
WantedBy=multi-user.target

Run these two commands
systemctl daemon-reload
systemctl enable poktnode.service
systemctl start poktnode.service

Other commands you will need
sudo systemctl stop poktnode.service
systemctl status poktnode.service

I.  Setup firewall


sudo apt-get install ufw  
sudo ufw default deny incoming  
sudo ufw default allow outgoing  
sudo ufw allow ssh   
sudo ufw allow 443
sudo ufw allow 80  
sudo ufw allow 8081
sudo ufw allow 26656    
sudo ufw enable  
sudo ufw status verbose  

J. Run command pocket query height
Compare on https://explorer.pokt.network/
wait till it syncs and then setup the validator

See Ben Van's 2nd Step to create the validator 
https://github.com/BenVanGithub/pokt-validator-configurator/blob/master/README_2.md




