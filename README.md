# Hassbian configuration
This document is step-by-step configuration guide for Hassbian preparation and configuration
to make external access to HASS through Dataplicity, use MySQL for Recorder and bridge with CloudMQTT 

1. MySQL install

         sudo apt-get install mysql-server
         sudo apt-get install libmysqlclient-dev
         sudo apt-get install mysql-client

2. Config MySQL encoding and collation to support non-english characters
   
sudo nano /etc/mysql/conf.d/mysql_ha.cnf
   
   [mysqld]

   init_connect='SET collation_connection = utf8_unicode_ci'
   init_connect='SET NAMES utf8 COLLATE utf8_unicode_ci'
   character-set-server = utf8
   collation-server = utf8_unicode_ci

   [client]
   
   default-character-set = utf8
   
sudo systemctl restart mysql.service
   
3. Create HASS database

mysql -u root -p
   
   CREATE DATABASE hass_db CHARACTER SET utf8 COLLATE utf8_unicode_ci;
   CREATE USER 'hassuser'@'localhost' IDENTIFIED BY 'XXX';   <-------XXX is your password
   GRANT ALL PRIVILEGES ON hass_db.* TO 'hassuser'@'localhost';
   
sudo su -s /bin/bash homeassistant
source /srv/homeassistant/bin/activate
pip3 install --upgrade mysqlclient
   
4. Install mosquitto

sudo ./home/pi/hassbian-scripts/install_mosquitto.sh

5. Config bridge with couldmqtt

sudo systemctl stop mosquitto.service
cd /etc/mosquitto/conf.d
sudo nano bridge.conf

   connection couldmqtt
   address <your cloudmqtt account>
   topic # in 1
   try_private true
   notifications false
   start_type automatic
   remote_clientid <your client id>
   remote_username <your user name>
   remote_password <your password>
   keepalive_interval 300
   cleansession true
   bridge_protocol_version mqttv311
   local_clientid hass
   bridge_cafile /etc/ssl/certs/ca-certificates.crt
   bridge_insecure false
   
Add line in your main Mosquitto config in "External config files" section
   
   include_dir /etc/mosquitto/conf.d
   
6. Install nginx

sudo apt-get install nginx
   
7. Configure NGINX

Adding line in nginx.conf in http section

sudo nano /etc/nginx/nginx.conf
Add folloving lines

   map $http_upgrade $connection_upgrade {
          default upgrade;
          ''      close;
        }

Create new config file

sudo nano /etc/nginx/sites-enables/hass

server {
         server_name _;
         listen 80 default_server;
         listen [::]:80 default_server ipv6only=on;
         proxy_buffering off;
         location / {
             proxy_pass http://localhost:8123;
             proxy_set_header Host $host;
             proxy_redirect http:// http://;
             proxy_http_version 1.1;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             proxy_set_header Upgrade $http_upgrade;
             proxy_set_header Connection $connection_upgrade;
             }
            }
            
Enable homeassistant configuration

cd /etc/nginx/sites-enabled
sudo unlink default
sudo ln ../sites-available/hass default

8. Install Dataplicity agent

curl https://dataplicity.com/<ACCOUNT_MAGIC_CODE>.py | sudo python
   
9. Enable wormhole

   What is wormhole - http://docs.dataplicity.com/docs/host-a-website-from-your-pi
   
10. Check your HASS page through public dataplicity address and through local network. 



   
   
   

   
