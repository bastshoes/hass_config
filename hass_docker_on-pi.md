1. Burn fresh Rasbian image on your SD card.
2. Create a simple empty file named ssh on the Windows-accessible part of the SD card after writing Raspbian to it.
3. Boot RaspberryPi and Pi using raspi-config.
   ```
   sudo raspi-config
   ```
4.Set static IP address for your PI
  ```
  sudo nano /etc/dhcpcd.conf 
  ```
  Add follofing lines to your file (change IP according to your network)
  ```
  interface eth0
  static ip_address=192.168.1.100/24
  static routers=192.168.1.1
  static domain_name_servers=192.168.1.1 8.8.8.8
  ```
5. Upgrade OS
  ```
  sudo apt-get update
  sudo apt-get upgrade -y
  sudo apt-get install raspberrypi-kernel
  ```
6. Set a Strong Password for default account (default is raspberry and everyone knows it)
  ```
  passwd
  ```
7. Change username(from pi to something else)

   - Change the password for root
   ```
   sudo passwd root
   ```
   - Enable root account to logon to SSH
   ```
   nano /etc/ssh/sshd_config
   ```
   
   - Add an entry to allow root logon via ssh
   ```
   PermitRootLogin yes
   ```
   - Save sshd_config [ctrl]+x,[enter],[enter]
   - Log off and back in with username root and the password you just created for it. Need 60 sec that ssh config file to be reread.
   - Modify the pi account to new username
   ```
   usermod -l hassadmin -d /home/hassadmin -m pi
   ```
   - Log off and log back on as your new user name
   - Lock the root account
     ```
     sudo passwd -l root
     ```
     (locks root again)

   - Disable root accounts ability to logon via SSH
     ```
     sudo nano /etc/ssh/sshd_config
     ```
   - Change ```PermitRootLogin``` to ```no```  

   - Turn off sudo password check for the new user name
     ```
     sudo nano /etc/sudoers.d/010_pi-nopasswd
     ```
   - Change the username pi to new username in file
   - Save file
   - Rename sudo file to indicate new sudo username
     ```
     sudo mv /etc/sudoers.d/010_pi-nopasswd /etc/sudoers.d/010_haadmin-nopasswd
     ```
   - DISABLE IPV6
     ```
     sudo nano /etc/sysctl.conf
     net.ipv6.conf.all.disable_ipv6=1
     ```
     save sysctl.conf file ([ctrl+X],Y,[enter],[enter])
     commit changes to the system
     ```
     sudo sysctl -p
     ```
8. Prepare USB for storing files with heavy write options
   - Detect your USB drive
   ```
   sudo dmesg|tail -n 20|grep sd
   ```
   - Partition the disk into two. One partition will hold the system logs, the other will hold your Home Assistant config
   ```
   sudo parted /dev/sda
   ```
   - Remove all partitions from USB drive
   ```
   rm 1
   ```
   - We'll allocate half of this for the logs, and half of it for the database with the following commands:
   ```
   mkpart primary 2048s 50%
   mkpart primary 50% 100%
   quit
   ```
   - Now you'll have 2 partitions /dev/sda1 and /dev/sda2, but they need a file system
   ```
   sudo mkfs -t ext4 /dev/sda1
   sudo mkfs -t ext4 /dev/sda2
   ```
  - To use these file systems, edit /etc/fstab
  ```
  sudo nano /etc/fstab
  ```
  Add the following lines. Replace the parts starting deadbeef-cafe (the UUIDs) with the UUID from previous step.
  ```
  UUID=deadbeef-cafe-dead-beef-cafef00f1234    /var/log    ext4    defaults,noatime,nofail    0    0
  UUID=deadbeef-cafe-dead-beef-cafef00f5678    /mydockers    ext4    defaults,noatime,nofail    0    0
  ```
  Making use of /mydockers
  ```
  sudo mkdir /mydockers
  sudo mount /mydockers
  cd /mydockers
  mkdir homeautomation
  ```
  Create symbolic link in home dir
  ```
  cd ~
  ln -s /mydockers/homeautomation homeautomation
  ```
  - Moving the log files
  ```
  sudo mv /var/log /var/log-old
  sudo mkdir /var/log
  sudo mount /var/log
  sudo reboot
  ```
9. Install Docker
  ```
  curl -fsSL get.docker.com -o get-docker.sh
  sudo sh get-docker.sh
  ```
  
10. Add “your” user to “docker” group using the following command
   ```
   sudo usermod -aG docker hassadmin
   ```
11. Install docker-compose
   ```
   sudo pip install docker-compose
   ```
