---
title: "Openresty on Ubuntu 14.04"
description: "Ubuntu 14.04 從原始碼編譯安裝 OpenResty（Nginx + Lua）的完整步驟，並示範用 content_by_lua 與外部 Lua 檔案撰寫 Nginx 應用。"
date: 2014-08-05
categories: ['Linux', 'Nginx']
---


## Install
choose the latest version of Openresty  
I used ngx_openresty-1.7.2.1.tar.gz 

```shell
# install packages
sudo apt-get install libreadline-dev libpcre3-dev libssl-dev perl  

# get openresty package
wget http://openresty.org/download/ngx_openresty-1.7.2.1.tar.gz   

# unzip
tar xzvf ngx_openresty-1.7.2.1.tar.gz

# install
cd ngx_openresty-1.7.2.1/

# You can setup with ./configure --prefix="the folder you want to install", default is '/usr/local/openresty'
./configure 
make
sudo make install # need permission to copy file to target folder
```

Default folder: `/usr/local/openresty`

## Setup 

create work folder in ~  
```
mkdir ~/work
cd ~/work
mkdir logs/ conf/
```

create a new file in `~/work/conf/nginx.conf`
```
worker_processes  1;
error_log logs/error.log;
events {
    worker_connections 1024;
}
http {
    server {
        listen 8080;
        location / {
            default_type text/html;
            content_by_lua '
                ngx.say("<p>hello, world</p>")
            ';
        }
    }
}
```

because Openresty is installed in `/usr/local/openresty`,  
we need to add the directory to PATH variable
```
PATH=/usr/local/openresty/nginx/sbin:$PATH
export PATH
```

Then use the way below to start nginx server
```
nginx -p `pwd`/ -c conf/nginx.conf
```

Assume that you have a nginx server originally, and then you installed Openresty, it can be set up at the same time if the port is not confilcted to the origin nginx. 

## Use external lua file to keep conf clean

Edit `~/work/conf/nginx.conf`  

```
worker_processes  1;
error_log logs/error.log;
events {
    worker_connections 1024;
}
http {
    server {
        listen 8080;
        location /hello {
            content_by_lua_file conf/hello.lua;
        }
    }
}
```


```lua
-- hello.lua
ngx.say("Hello World")
```
