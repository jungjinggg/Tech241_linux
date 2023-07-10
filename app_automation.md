# Automation L1

## Start the Sparta app with a script (no database)

### Sparta App front page
1) update - gets the information about the latest veriosn of packages available for the system (doesn't download or install anything).
   ```bash
   sudo apt update -y
   ```
2) upgrade - download and install the latest version of the packages for the system.
   ```bash
   sudo apt upgrade -y
   ```
3) install nginx - web server (after it installed, it starts autometically)
   ```bash
   sudo apt install nginx -y
   ```
4) enable nginx
   ```bash
   sudo systemctl enable nginx
   ```
5) (optional) check the status of nginx
   ```bash
   sudo systemctl status nginx
   ```
6) download nodejs - for running web applications
   ```bash
   curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - 
   ```
7) install nodejs
   ```bash
   sudo apt install nodejs -y
   ```
8) Install node package manager pm2 to run nodejs in the background
   ```bash
   sudo npm install pm2 -g
   ``` 
9)  copy the app folder into vm
    ```bash
    git clone https://github.com/jungjinggg/tech241_sparta_app.git app
    ```
10) go into the app folder
    ```bash
    cd ~/app/app
    ```
11) inside the app folder run:
    1) install the dependencies in the local node_modules
         ```bash
         npm install 
         ```
    2) run the app
         ```bash
         pm2 start app.js
         ```

### Full script
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

# download nodejs
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# install nodejs
sudo apt install nodejs -y

# node package manager
sudo npm install pm2 -g

# copy app folder to VM
git clone https://github.com/jungjinggg/tech241_sparta_app.git app

# go into the app folder
cd ~/app/app

# install npm
npm install

# run sparta node app in the background
pm2 start app.js
```

## Running app in the background
1) using `node app.js &` it will run in the background but if the script cannot be executed again as the port 3000 is in use when run the script again.

2) using prefix `nohup node app.js &` it allows the script to run in the background even if the hang up (HUP) signal is sent.

3) using `pm2 start app.js`


# Automation L2

## Configure Mongo DB VM (including bindIp) with the script

### Sparta App posts page
1) update
   ```bash
   sudo apt-get update -y
   ```
2) upgrade
   ```bash
   sudo apt-get upgrade -y
   ```
3) download MongoDB key (make sure it's the right version: 3.2.x)
   ```bash
   wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -
   ```
4) add the MongoDB source to the package manager's configuration
   ```bash
   echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
   ```
5) update again
   ```bash
   sudo apt-get update -y
   ```
6) install MongoDB
   ``` bash
   sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
   ```
7) configure MongoDB to accept any connections
   change bindIP to 0.0.0.0
   ```bash
   sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/g' /etc/mongod.conf
   ```
8) start MongoDB (it doesn't start autometically like nginx)
   ```bash
   sudo systemctl start mongod
   ```
9) check MongoDB status
   ```bash
   sudo systemctl status mongod
   ``` 
10) enable MongoDB
    ```bash
    sudo systemctl enable mongod
    ```

### Full Script
```bash
#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# download key for the right version
wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -

# source list - specify mongo db version
echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

# update again
sudo apt update -y

# install mongo db
sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

# configure bindip to 0.0.0.0
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/g' /etc/mongod.conf

# start mongo db
sudo systemctl start mongod

# check the status
sudo systemctl status mongod

# enable mongo db
sudo systemctl enable mongod
```
*`sed` command -i is used to replace bindIP so that inbound port can be from any any ip address*

# Automation L3

## Modify app VM script to use database VM
At this level, we need to modify the script so that the app virtual machine can retrieve data from database virtual machine. The script stays almost the same as level 1 automation.


**Set environment variable in the script**
```bash
export DB_HOST=mongodb://172.187.161.179:27017/posts
```

### Full Script
```bash
#!/bin/bash

# update
sudo apt update -y

# upgrade
sudo apt upgrade -y

# install nginx - when installed it starts autometically
apt install nginx -y

# enable nginx - makes sure that when vm is restarted, ngix auto start on reboot
sudo systemctl enable nginx

# install nodejs
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# download nodejs
sudo apt install nodejs -y

# node package manager
sudo npm install pm2 -g

# copy app folder to VM
git clone https://github.com/jungjinggg/tech241_sparta_app.git repo

# env variable
export DB_HOST=mongodb://172.31.34.136:27017/posts

# make sure sed is installed
sudo apt install sed -y

# reverse proxy
sudo sed -i 's@try_files $uri $uri/ =404;@proxy_pass http://localhost:3000;@g' /etc/nginx/sites-available/default

# restart nginx
sudo systemctl restart nginx

# go into the app folder
cd repo/app

# install npm - install the nodejs code/ downloads required dependencies for nodejs, also check for DB_HOST, if it exists itll try to connect, it non exists, it wont set up posts page  
npm install

# seed batabase
echo "Clearing and seeding database.."
node seeds/seed.js
echo " --> Done!"

# kill previous app background process
pm2 kills

# run sparta node app in the background
pm2 start app.js
```
### Stop the app from running
```bash
pm2 stop 0
```

### Note
*To get the app working, the database vm needs to be run first to pull data from MongoDB. Then run the app vm so that it can create a post page with the data from database vm.*

*npm = node package manager*

*pm2 = advanced node process manager*


### App script without installing nginx (only run the app)
```bash
#!/bin/bash

# env variable
export DB_HOST=mongodb://172.31.34.136:27017/posts

# go into the app folder
cd repo/app

# install npm - install the nodejs code/ downloads required dependencies for nodejs, also check for DB_HOST, if it exists itll try to connect, it non exists, it wont set up posts page  
npm install

# seed batabase
echo "Clearing and seeding database.."
node seeds/seed.js
echo " --> Done!"

# run sparta node app in the background
pm2 start app.js
```