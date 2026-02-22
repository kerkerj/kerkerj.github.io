---
title: "Codeigniter on nginx"
description: "Complete Nginx + php5-fpm configuration for CodeIgniter 2.2.0 on Ubuntu 14.04, including removing index.php from URLs with proper Nginx config and config.php settings."
date: 2014-09-25
categories: ['PHP', 'Nginx']
---


I use Ubuntu 14.04, Nginx 1.4.6, php5-fpm, Codeigniter 2.2.0

It will remove index.php, and access Codeigniter site normally.

Clean configuration "/etc/nginx/site-enabled/default":

```text 
server {

#>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> START
        listen 80;
        listen [::]:80 default_server ipv6only=on;

        root /YOUR/PROJECT/ROOT;
        index index.html index.htm index.php;

        # Make site accessible from http://localhost/
        server_name localhost;
        
        #location ~* .(ico|css|js|gif|jpe?g|png)(?[0-9]+)?$ {
        #   expires max;
        #   log_not_found off;
        #}
        
        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ /index.php;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }

        location ~ .php$ {
            root           /YOUR/PROJECT/ROOT;
            try_files $uri =404;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
            fastcgi_buffer_size 128k;
            fastcgi_buffers 256 4k;
            fastcgi_busy_buffers_size 256k;
            fastcgi_temp_file_write_size 256k;
        }

#<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< END

        # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
        #location /RequestDenied {
        #       proxy_pass http://127.0.0.1:8080;

        #error_page 404 /404.html;

        # redirect server error pages to the static page /50x.html
        #
        #error_page 500 502 503 504 /50x.html;
        #location = /50x.html {
        #       root /usr/share/nginx/html;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #       fastcgi_split_path_info ^(.+\.php)(/.+)$;
        #       # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
        #
        #       # With php5-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
        #       fastcgi_pass unix:/var/run/php5-fpm.sock;
        #       fastcgi_index index.php;
        #       include fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}

```

Codeigniter config - config.php:

```php 
$config['index_page'] = '';
$config['uri_protocol'] = 'REQUEST_URI';
```
