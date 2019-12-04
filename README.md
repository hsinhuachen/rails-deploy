# AWS 安裝

## Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type

### log ssh
	chmod 400 keypair
	ssh -i keypair ec2-user@ip

### 新增使用者
	adduser hsinhua
	passwd hsinhua
	chmod u+w /etc/sudoers
	vim /etc/sudoers
	hsinhua ALL=(ALL) ALL
	sudo su hsinhua


#### SSH key login
	# mac 本地
	ssh-keygen -t rsa
	cat ~/.ssh/id_rsa.pub

會出現一段文字，複製下來

	$ server上
	ssh-keygen -t rsa

將剛才 Mac 本地命令行中，複製的那一段文字貼進去，然後按:wq保存離開

	vi /home/hsinhua/.ssh/authorized_keys
	chmod 644 /home/hsinhua/.ssh/authorized_keys
	sudo service ssh restart

### 伺服器準備工作
	sudo yum update
	ls /usr/share/zoneinfo
	ZONE="Asia/Taipei"
	# Time zone=>Asia=>Taiwan

	sudo yum install -y build-essential git-core bison openssl libreadline6-dev curl zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3  autoconf libc6-dev libpcre3-dev curl libcurl4-nss-dev libxml2-dev libxslt-dev imagemagick nodejs libffi-dev

### Install RVM
https://rvm.io/rvm/install

	\curl -sSL https://get.rvm.io | bash -s stable --ruby
	source /home/ec2-user/.rvm/scripts/rvm

### Install Ruby
	rvm install 2.5
	rvm --default use 2.5
	gem install bundler

### Install Node.js
	curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
	sudo yum install -y nodejs

### Install Rails
	gem install rails

### Install Nginx + Passenger
https://www.phusionpassenger.com/library/install/nginx/install/oss/rubygems_rvm/#step-1:-install-gem

	sudo yum install nginx
	sudo vi /etc/nginx/nginx.conf
	sudo /etc/init.d/nginx start
	sudo chown -R hsinhua:hsinhua /usr/share/nginx/html


	gem install passenger
	sudo yum remove nginx
	sudo rm -rf /etc/nginx

	passenger-install-nginx-module
	rvm sudo passenger-config validate-install
	rvm sudo passenger-memory-stats

### Rails Setting
path: /usr/share/nginx/html

	git clone git@github.com
	cd rais
	bundle install
	EDITOR="mate --wait" bin/rails credentials:edit
	rails db:migrate RAILS_ENV=production
	rails s -e production
	rails console production


### Configuring Nginx and Passenger
	passenger-config about ruby-command
	passenger start -a 0.0.0.0 -p 3000 -d -e production

	sudo nano /etc/nginx/sites-enabled/myapp.conf

	server {
	    listen 80;
	    server_name ip;
			
	    root /var/www/app/oniondesign-web/public;
			
	    # Turn on Passenger
	    passenger_enabled on;
	    # passenger_ruby /path-to-ruby;
			
	    passenger_min_instances 1;
				
		location ~ ^/assets/ {
		   expires 1y;
		   add_header Cache-Control public;
		   add_header ETag "";
		   break;
		}
		
		# second app
		location ~ ^/onion(/.*|$) {
            alias /var/www/shop/oniondesign-web/public$1;
            passenger_base_uri /onion;
            passenger_app_root /var/www/shop/oniondesign-web;
            passenger_document_root /var/www/shop/oniondesign-web/public;
            passenger_enabled on;
        }
	}


	#root user
	sudo nano /etc/nginx/nginx.conf
	service nginx restart

passenger_root /home/hsinhua/.rvm/gems/ruby-2.5.3/gems/passenger-6.0.2/bin/passenger;
passenger_ruby /home/hsinhua/.rvm/gems/ruby-2.5.3/wrappers/ruby;

### Install Capistrano
	
	# gemfile
	gem 'capistrano', "~> 3.10", :group => :development
	gem 'capistrano-rails', :group => :development
	gem 'capistrano-passenger', :group => :development

	bundle install
	bundle exec cap install

### Sass re compile
	rake assets:precompile

#### Mac 本地 
	cap production deploy


#### passenger 指令
https://www.phusionpassenger.com/library/walkthroughs/basics/ruby/passenger_command.html

- bundle exec passenger start
- bundle exec passenger stop
- passenger-config restart-app
- rvmsudo passenger-memory-stats
- passenger start -a 0.0.0.0 -p 3000 -d -e production



