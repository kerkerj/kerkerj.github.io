---
title: "Capistrano with rails - basic notes"
description: "Capistrano 3 搭配 Rails 的自動化部署入門筆記，說明 deploy.rb 和 production.rb 的關鍵設定，從 GitHub 拉取程式碼並部署到遠端伺服器。"
date: 2014-08-05
slug: capistrano-with-rails-basic-notes
tags: ['rails', 'devops']
---


詳細的東西還是看 project's github page 比較快 - [Capistrano@github](https://github.com/capistrano/capistrano)

Capistrano 剛開始寫 deploy script 時真的會有點搞不太懂 XD  
記錄一下使用 'capistrano' 把特定的 github repo 抓到 remote server  

## 安裝

先在 Gemfile 加入:

```
gem 'capistrano', '~> 3.2.0'   
``` 
然後安裝~

```
bundle install
```

步驟大概會是：

假設已經寫完 capistrano 了，執行 script 時，  
capistrano 會先利用 script 裡提供的 server ip 以及 public key，  
先連線到 remote server，接著再到 github 上拉 code 到指定的目錄裡，  
再重開 server。

## 產生相關檔案

```
bundle exec cap install 
```

會產生以下檔案:  (copy from [Capistrano@github](https://github.com/capistrano/capistrano))

```
├── Capfile
├── config
│   ├── deploy
│   │   ├── production.rb
│   │   └── staging.rb
│   └── deploy.rb
└── lib
    └── capistrano
            └── tasks
```

deploy.rb 通常是設定 source control 的類型 (可以設定 git, hg, svn)，   
project repo 的路徑，要 deploy 到 server 的哪個目錄等等，  
`config/deploy/*` 底下的檔案則是根據不同的 stage 分別設定，  
以下是 deploy.rb, production.rb 的一些簡單設定 

```ruby 
# config valid only for Capistrano 3.1
lock '3.2.1'

set :application, 'application_name'
set :repo_url, 'github_repo_url'

set :deploy_via, :copy

# Default branch is :master
# ask :branch, proc { `git rev-parse --abbrev-ref HEAD`.chomp }.call

# Default deploy_to directory is /var/www/my_app
set :deploy_to, '/server/www/path/'

# Default value for :scm is :git
set :scm, :git

# Default value for :format is :pretty
set :format, :pretty

# Default value for :log_level is :debug
set :log_level, :debug

# Default value for :pty is false
# set :pty, true

# Default value for :linked_files is []
# set :linked_files, %w{config/database.yml}

# Default value for linked_dirs is []
# set :linked_dirs, %w{bin log tmp/pids tmp/cache tmp/sockets vendor/bundle public/system}

# Default value for default_env is {}
# set :default_env, { path: "/opt/ruby/bin:$PATH" }

# Default value for keep_releases is 5
# set :keep_releases, 5

namespace :deploy do

  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      # Your restart mechanism here, for example:
      execute :mkdir, '-p', "#{ release_path }/tmp"
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end

  after :publishing, :restart

  after :restart, :clear_cache do
    on roles(:web), in: :groups, limit: 3, wait: 10 do
      # Here we can do anything such as:
      # within release_path do
      #  execute :rake, 'cache:clear'
      # end
    end
  end

end

```

production.rb:

```ruby 
# Simple Role Syntax
# ==================
# Supports bulk-adding hosts to roles, the primary server in each group
# is considered to be the first unless any hosts have the primary
# property set.  Don't declare `role :all`, it's a meta role.

role :app, %w{app@server}
role :web, %w{web@server}
role :db,  %w{db@server}


# Extended Server Syntax
# ======================
# This can be used to drop a more detailed server definition into the
# server list. The second argument is a, or duck-types, Hash and is
# used to set extended properties on the server.

server 'server_ip', user: 'server_user', roles: %w{app}, my_property: :my_value


# Custom SSH Options
# ==================
# You may pass any option but keep in mind that net/ssh understands a
# limited set of options, consult[net/ssh documentation](http://net-ssh.github.io/net-ssh/classes/Net/SSH.html#method-c-start).
#
# Global options
# --------------
#  set :ssh_options, {
#    keys: %w(/home/rlisowski/.ssh/id_rsa),
#    forward_agent: true
#    auth_methods: %w(password)
#  }
#
# And/or per server (overrides global)
# ------------------------------------
 server 'server_ip',
   user: 'server_user',
   ssh_options: {
     keys: %w(/user/.ssh/id_rsa),
     forward_agent: false,
     auth_methods: %w(publickey password)
   }

```

設定完後，因為這邊是設定 production.rb  
因此執行以下指令 deploy 到 production server
```shell
$ bundle exec cap production deploy
```  

如果 `exit status 0 (successful).`   
那就是成功啦~
