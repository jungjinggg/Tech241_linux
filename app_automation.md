# Automation L1

## Start the Sparta app with a script (no database)
```bash
#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# install nginx - when installed it starts autometically
sudo apt install nginx -y

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

## Running app in the background
1) using `node app.js &` it will run in the background but if the script cannot be executed again as the port 3000 is in use when run the script again.

2) using prefix `nohup node app.js &` it allows the script to run in the background even if the hang up (HUP) signal is sent.

3) using `pm2 start app.js`

when start terminal when run command you can start 

everytime you run a script u specify /bin/bash tell linux to run this script to run another bash shell

# Automation L2

## Configure Mongo DB VM (including bindIp) with the script
```bash
#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# download key for the right version
wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -

# source list - specify mongo db version
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /e$

# update again
sudo apt update -y

# install mongo db
sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20$

# configure bindip to 0.0.0.0
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/g' /etc/mongod.conf

# start mongo db
sudo systemctl start mongod

# check the status
sudo systemctl status mongod

# enable mongo db
sudo systemclt enable mongod
```

`sed` command -i is used to replace bindIP so that inbound port can be from any ports

# Automation L3
## Modify app VM script to use database VM 
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

# env variable
export DB_HOST=mongodb://172.187.161.179:27017/posts

# go into the app folder
cd /home/adminuser/app/app

# install npm
npm install

# run sparta node app in the background
pm2 start app.js
```
## Stop pm2
`pm2 stop 0`

### Note
*To get the app working, the database vm needs to be run first to pull data from MongoDB. Then run the app vm so that it can create a post page with the data from database vm.*