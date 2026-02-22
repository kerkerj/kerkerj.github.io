---
title: '[Vagrant] use knife-solo to deploy a new VM'
description: "My own computer: host side My VM: guest side knife-solo is a tool for Chef. host side: you need ruby environment! $ gem install knife-solo The only..."
date: 2013-12-19
categories: 
---


My own computer: host side
My VM: guest side

knife-solo is a tool for Chef.
host side: you need ruby environment!
```
$ gem install knife-solo
```

The only thing you need to install is just knife-solo by ruby!

```
$ knife configure -r . --defaults (產生預設設定檔，內容為空)
$ knife cookbook create "package"
$ knife solo bootstrap vagrant@ip.address (bootstrap = prepare + cook, only for the first time)
```

加入菜單：

```
$ knife cookbook create nginx
```

在 cookbooks/nginx/recipes/default.rb 裡最少要加入
`package 'nginx' ` 也就是套件名稱
其他內容可以自己加
例如：

```
package 'nginx'

service 'nginx' do
    supports [:status, :restart, :reload]
      action :start
end

directory "/vagrant/demo" do
end

file "/vagrant/index.php" do
    content "<?php phpinfo(); ?>"
end

template "/etc/nginx/sites-enabled/default" do
    source 'default.erb'
      notifies :restart, 'service[nginx]', :immediately
end

```
