# Automation L1
## Start the Sparta app with a script (no database)
```bash
#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# install nginx - when installed it starts autometically
apt install nginx -y

# restart nginx
sudo systemctl restart nginx

# enable nginx - makes sure that when vm is restarted, ngix auto start on reboot
sudo systemctl enable nginx

# install nodejs
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# download nodejs
sudo apt install nodejs -y

# node package manager
sudo npm install pm2 -g

# copy app folder to VM
git clone https://github.com/jungjinggg/tech241_sparta_app.git app

# go into the app folder
cd /home/adminuser/app/app

# install npm
npm install

# run sparta node app in the background
pm2 start app.js
```

