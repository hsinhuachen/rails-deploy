# AWS Bug 整理

### 'find_spec_for_exe': can't find gem bundler (>= 0.a) with executable bundle
	gem update --system
	bundle install

### rbenv_ruby is not set; ruby version will be defined by the remote hosts via rbenv
https://github.com/capistrano/rbenv

### Could not load the 'listen' gem. Add gem 'listen' to the development group of your Gemfile (LoadError)
	bundle config --delete without
	bundle config --delete with
	bundle install

### WARNING:root:could not open file '/etc/apt/sources.list.d/passenger.list'
	sudo chmod 644 /etc/apt/sources.list.d/passenger.list

### Invalid gemspec in [/usr/share/rubygems-integration/all/specifications/rbnacl-libsodium.gemspec]: stack level too deep
Warning: Running `gem pristine --all` to regenerate your installed gemspecs (and deleting then reinstalling your bundle if you use bundle --path) will improve the startup performance of Spring.
	
### in `rescue in _decrypt': ActiveSupport::MessageEncryptor::InvalidMessage (ActiveSupport::MessageEncryptor::InvalidMessage)


### git: error: Pulling is not possible because you have unmerged files.
	git reset --hard FETCH_HEAD

