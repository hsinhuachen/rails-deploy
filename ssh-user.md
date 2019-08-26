### 新增使用者
	adduser hsinhua --ingroup sudo
	sudo su hsinhua
	sudo chown -R hsinhua:hsinhua /var/www/

### SSH key login

#### Mac 本地
	ssh-keygen -t rsa
	cat ~/.ssh/id_rsa.pub

會出現一段文字，複製下來

#### Server上
	ssh-keygen -t rsa

將剛才 Mac 本地命令行中，複製的那一段文字貼進去，然後按:wq保存離開

	vi /home/hsinhua/.ssh/authorized_keys
	chmod 644 /home/hsinhua/.ssh/authorized_keys
	sudo service ssh restart

## Install passenger
https://www.phusionpassenger.com/library/install/nginx/install/oss/bionic/

#### Mac 本地
	cat ~/.ssh/id_rsa.pub
	EDITOR="mate --wait" bin/rails credentials:edit

#### Server
	cd ~/shared/config
	vim database.yml
	vim master.key
	
#### Mac 本地 
	cap production deploy
