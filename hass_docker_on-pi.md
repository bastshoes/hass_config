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
   usermod -l newname -d /home/newname -m pi
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

  
