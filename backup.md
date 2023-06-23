## Provision.sh back up
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

# start nginx if its not running
# sudo systemctl start nginx

```