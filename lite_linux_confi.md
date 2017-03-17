1. Force boot into terminal

		sudo nano /etc/default/grub
		
		GRUB_CMDLINE_LINUX_DEFAULT="text"
		GRUB_CMDLINE_LINUX="text"
		GRUB_TERMINAL=console
		
		sudo update-grub
		
		sudo systemctl set-default multi-user.target
		
		sudo systemctl set-default graphical.target
		
		sudo systemctl disable display-manager.service

2. Install ssh

		sudo apt-get install ssh
		
3. Allow ssh in

		sudo ufw allow ssh
		
# https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-14-04

4. Allow mysql in
		
		sudo ufw allow from 15.15.15.0/24 to any port 3306
		
