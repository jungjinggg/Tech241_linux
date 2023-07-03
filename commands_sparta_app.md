# Sparta App (Manually)

The app is nodejs app and works on port 3000. It has 2 different features:

1) **Front web page** (no database need to get this to run)
2) **Database posts** (shows the data from the database)

## Allow port 3000 on azure portal networking
1) Add inbound port rule
2) Enter destination port ranges: 3000
3) Select TCP protocal
4) Add desciption (optional)

## Requirements for running Sparta App
1) Linux VM - Ubuntu 18.04 LTS
2) Update and upgrade
   ```sudo apt update -y```
   ```sudo apt upgrade -y```
3) Web server - nginx (dependency)
4) Right version nodejs - version 12.x (dependency)
    
    On your home directory 

    1) Download nodejs
    ```
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
    ```

    2) Install nodejs
    ```
    sudo apt install nodejs -y
    ```

    3) Install node package manager pm2 to run nodejs in the background (not critical atm)
    ```
    sudo npm install pm2 -g
    ```
5) Get app folder to the app VM
   ```git clone <repo_url>```
6) If need posts page, connect to db set DB_HOST env variable 
   
   ```export DB_HOST=mongodb://<vm_ip_address>:27017/posts```
   
   ```export DB_HOST=mongodb://172.187.176.58:27017/posts```   
7) In the app folder:
    1) Navigate to inside the app folder to install the app
    ```
    npm install
    ```

    2) Start the app
   
    ```npm start``` or ``` node app.js```

## Stop the app
### shut down the app from running
```ctrl c```

### 2 servers cannot use the same port so using ctrl z, means port 3000 still occupying it. In order to start the app again:
```kill -1 <PID>```

```kill <PID>```

```kill -9 <PID>```

## Requirements for running database VM
1) Linux VM - Ubuntu 18.04 LTS
2) Update and upgrade
3) Install Mongo Database - version 3.2.x
   
    1) download key for the right version
    ```bash
    wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | sudo apt-key add -
    ```
    2) Source list - specify mongo database version
    ```bash
    echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    ```
    3) update again
    ```bash
    sudo apt update -y
    ```
    4) install Mongo DB
    ``` bash
    sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
    ```

4) Configure mongodb to accept connections from app VM
   
   1) Change bindIp to accept any IP address (This is for testing only)
   ```
   sudo nano /etc/mongod.conf
   ```
   Locate network interfaces and change bindIp to 0.0.0.0
   ```bash
   # network interface
   net:
    port: 27017
    bindIp: 0.0.0.0
   ```
   2) Start Mongo batabase 
   ```
   sudo systemctl start mongod
   ```
   3) Check the status
   ```
   sudo systemctl status mongod
   ```
   4) Enable Mongo database
   ```
   sudo systemctl enable mongod
   ```