---
title: '[Ubuntu] RoR environment'
description: "Complete guide to setting up Ruby 2.0 + Rails 4 + Passenger Nginx + php5-fpm + MySQL on Vagrant Ubuntu 12.04 using RVM, with step-by-step commands and configuration."
date: 2013-12-20
slug: ubuntu-ror-environment
tags: ['mysql', 'linux', 'ruby', 'nginx', 'devops', 'php']
---


## RVM with Ruby2.0.0-p353 + Rails4.0.2 + Ubuntu12.04(precise64) + php-fpm by Vagrant(clean install)

p.s. 
If you don't use RVM, you can just follow the instruction from [Passenger offcial website](http://www.modrails.com/documentation/Users%20guide%20Nginx.html#install_on_debian_ubuntu). It will be easier.
Plus, I installed all these things by Vagrant.

## First thing to do

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

## Pre-setup: install necessary packages

```
$ sudo apt-get update
$ sudo apt-get install build-essential libssl-dev libpcre3-dev libncurses5-dev libreadline6-dev git vim curl libcurl4-openssl-dev libreadline6 autoconf openssl git-core zlib1g zlib1g-dev  libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev autoconf libc6-dev libgdbm-dev libncurses5-dev automake libtool bison subversion pkg-config libffi-dev
```

## Get .bashrc from my repo (Optional) 

```
$ cd
$ git clone https://github.com/jerry54010/bash-it.git
$ mv bash-it .bash_it
$ cd .bash_it
$ git checkout ubuntu
$ ln -s ~/.bash_it/.bash_profile ~/.bash_profile
$ cd
$ source ~/.bash_profile
```

## Get .vimrc from my repo (Optional)

```
$ wget https://gist.github.com/jerry54010/8049575/raw/42ced22651fedf06174457e311d22d17f6591b65/.vimrc
```

## RVM and Ruby 2.0 (RVM: Ruby version management)

```
$ \curl -sSL https://get.rvm.io | bash -s stable
$ source ~/.bash_profile //if you installed my bash_profile. 
//If not, source ~/.profile
$ rvm requirements
$ echo 'gem: --no-ri --no-rdoc'  >> ~/.gemrc
$ rvm install 2.0.0
$ source ~/.bash_profile //if you installed my bash_profile. 
//If not, source ~/.profile

add this line to .bash_rc or .bash_profile (if you don't use my .bash_profile)
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function

$ rvm use 2.0.0 --default
```

Now we should be using ruby2.0.0 by RVM, let's check:

```
$ rvm list
=* ruby-2.0.0-p353 [ x86_64 ] 
// => - current
// =* - current && default
//  * - default
```

## Nodejs, libv8-dev(for javascript) and SQLite3 (sqlite3 libsqlite3-dev)

```
$ sudo apt-get -y install nodejs libv8-dev sqlite3 libsqlite3-dev 
```

## Passenger with NGINX

```
$ gem install passenger 
$ rvmsudo passenger-install-nginx-module
choose 2: customize
	a. src dir: /home/vagrant/nginx-1.4.4/ (where the nginx you download)
	b. I installed nginx with passenger in `/etc/nginx` (default is /opt/nginx)
	c. default
```

## Get nginx script 
If you use /opt/nginx as root dir, you don't need to modify this script.
If you use /etc/nginx or else dir as root dir like me, you should modify the script.
(from [linode](https://library.linode.com/web-servers/nginx/installation/ubuntu-12.04-precise-pangolin))

```
$ wget -O init-deb.sh http://library.linode.com/assets/1139-init-deb.sh
$ sudo mv init-deb.sh /etc/init.d/nginx
$ chmod +x /etc/init.d/nginx
$ sudo /usr/sbin/update-rc.d -f nginx defaults
```
The nginx default public folder will be in `/etc/nginx/html` 
check nginx version:

```
$ /etc/nginx/sbin/nginx -v
nginx version: nginx/1.4.4
```

## Install php5-fpm

```
$ sudo apt-get -y install php5-cli php5-common php5-fpm
$ sudo vim /etc/php5/fpm/php.ini 
//(find and change to `cgi.fix_pathinfo = 0` )
$ sudo vim /etc/php5/fpm/pool.d/www.conf 
//change: listen = 127.0.0.1:9000 to listen = /var/run/php5-fpm.sock
$ sudo vim /etc/nginx/conf/nginx.conf or /opt/nginx/conf/nginx.conf //find and modify 
location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
    # With php5-cgi alone:
    #fastcgi_pass 127.0.0.1:9000;
    # With php5-fpm:
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include fastcgi_params;
}
$ echo "<?php phpinfo(); ?>" | sudo tee -a /etc/nginx/html/phpinfo.php
```

Restart all:

```
$ sudo service php5-fpm restart
$ sudo service nginx restart
```

## Install rails

```
$ gem install rails
```

## RoR environment check

```
$ ruby -v 
ruby 2.0.0p353 (2013-11-22 revision 43784) [x86_64-linux]
$ rails -v
Rails 4.0.2
$ rake -V
rake, version 10.1.0
```

If you passed, you are good to go!

## Deploy Rails application by nginx:
Assume your rails application path is: `/home/vagrant/projects/subapp/`
(by `rails new subapp`)
then, the `/etc/nginx/conf/nginx.conf` will be:

```
http {
    ...
    server {
        listen 80;
        server_name localhost;
        root /etc/nginx/html;

        # This block has been added.
        location ~ ^/subapp(/.*|$) {
            passenger_base_uri /subapp;
            alias /home/vagrant/projects/subapp/public$1;  # <-- be sure to point to 'public'!
            passenger_app_root /home/vagrant/projects/subapp;
            passenger_enabled on;
            rails_env development; # or production
        }
    }
    ...
}
```

restart nginx, you can access the url: `http://localhost/subapp/`  

## Install MySQL and Mysql adapter  

```
$ sudo apt-get -y install mysql-server mysql-common mysql-client libmysqlclient-dev  
$ gem install mysql2 //gem mysql2 is faster than gem mysql  
```

>The gem you installed is placed in RVM's ruby, if you are using RVM's ruby.   

If you use MySQL in your rails application, remember to change the database.yml.   

p.s. If you want to use phpmyadmin, you can do:  

``` 
$ sudo apt-get install php5-mysql
```

download phpmyadmin, then extract to `/etc/nginx/html/phpmyadmin`  
then start the server, access: `http://localhost/phpmyadmin/`  
