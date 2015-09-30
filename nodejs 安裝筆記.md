#Install node.js v4.1.1 on Ubuntu 14.04

	sudo apt-get remove nodejs
	curl --silent --location https://deb.nodesource.com/setup_4.x | sudo bash -
	sudo apt-get install --yes nodejs

check:

	node -v
	npm -v
	
	hanshuang@VirtualBox:~$ node -v
	v4.1.1
	hanshuang@VirtualBox:~$ npm -v
	2.14.4
