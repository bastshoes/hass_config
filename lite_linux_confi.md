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
