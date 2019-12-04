# rails deploy 安裝

Ubuntu Server
https://ihower.tw/rails/deployment.html

## SSH
		chmod 400 keypair
		ssh -i keypair ubuntu@[YOUR_IP_ADDRESS]

### 新增使用者
	adduser hsinhua
	passwd hsinhua
	chmod u+w /etc/sudoers
	vim /etc/sudoers
	hsinhua ALL=(ALL) ALL
	sudo su hsinhua

## Install Ruby

### System configuration
	sudo apt-get update
	sudo apt-get upgrade -y
	sudo dpkg-reconfigure tzdata

#### 安裝套件
	sudo apt-get install -y build-essential git-core bison openssl libreadline6-dev curl zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3  autoconf libc6-dev libpcre3-dev curl libcurl4-nss-dev libxml2-dev libxslt-dev imagemagick nodejs libffi-dev

### Install RVM
https://rvm.io/rvm/install

	\curl -sSL https://get.rvm.io | bash -s stable --ruby
	source /usr/local/rvm/scripts/rvm

### Install Ruby
	rvm install 2.5
	rvm --default use 2.5
	gem install bundler	

### Install Nodejs
	sudo apt install nodejs

### Install Rails
	gem install rails

### Install Nginx + Passenger
https://www.phusionpassenger.com/library/install/nginx/install/oss/bionic/
	
	sudo apt-get install -y dirmngr gnupg
	sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
	sudo apt-get install -y apt-transport-https ca-certificates
		
	sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'
	sudo apt-get update
				
	sudo apt-get install -y libnginx-mod-http-passenger		

	passenger-install-nginx-module
	rvmsudo passenger-config validate-install
	rvmsudo passenger-memory-stats
