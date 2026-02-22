---
title: "Install rails server env with rbenv on Ubuntu 14.04"
description: "Ubuntu 14.04 使用 rbenv 安裝 Ruby 2.1 + Rails 4 + Nginx + Phusion Passenger + MySQL 的完整生產環境建置教學，含各步驟指令。"
date: 2014-07-30
categories: ['Linux', 'Rails', 'Ruby', 'Nginx', 'DevOps', 'MySQL']
---


```
nodejs v0.10.25 (for rails javascript engine)

rbenv 0.4.0-98-g13a474c

ruby 2.1.2p95 (2014-05-08 revision 45877) [x86_64-linux]

Rails 4.1.4

nginx version: nginx/1.6.0

Phusion Passenger version 4.0.48

mysql  Ver 14.14 Distrib 5.5.38, for debian-linux-gnu (x86_64) using readline 6.3

```

## Installing Ruby & Rails

```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties nodejs npm
```

Use rbenv

```
cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 2.1.2
exec $SHELL

rbenv global 2.1.2
ruby -v
```

Don't install rdoc when installing gems

```
echo "gem: --no-ri --no-rdoc" > ~/.gemrc
```

Install rails

```
gem install rails
exec $SHELL
rails -v
```

## Installing Nginx with passenger

```
# Install Phusion's PGP key to verify packages
gpg --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7
gpg --armor --export 561F9B9CAC40B2F7 | sudo apt-key add -

# Add HTTPS support to APT
sudo apt-get install apt-transport-https

# Add the passenger repository
sudo sh -c "echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main' >> /etc/apt/sources.list.d/passenger.list"
sudo chown root: /etc/apt/sources.list.d/passenger.list
sudo chmod 600 /etc/apt/sources.list.d/passenger.list
sudo apt-get update

# Install nginx and passenger
sudo apt-get install nginx-full passenger
```

Edit passenger configuration in `/etc/nginx/nginx.conf`

```
##
# Phusion Passenger
##
# Uncomment it if you installed ruby-passenger or ruby-passenger-enterprise
##

passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;

passenger_ruby /usr/bin/ruby; #change this line

# passenger_ruby /home/yourpath/.rbenv/shims/ruby; # If you use rbenv
# Use `which ruby`
```

## Installing MySQL

```
sudo apt-get install mysql-server mysql-client libmysqlclient-dev
```

Done!
