# hass_config
This document is step-by-step configuration guide for HASS and various components

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

   

   
