---
title: '[Ubuntu] [RoR] install with rvm'
description: "Ubuntu 上使用 RVM 安裝 Ruby on Rails 的完整流程，涵蓋 RVM 指令、Gem 管理、Gemset 使用，並附上 OpenSSL、zlib、ExecJS 三種常見錯誤的排解方法。"
date: 2012-04-27
slug: ubuntu-ror-install-with-rvm
tags: ['ruby', 'linux', 'rails']
---


以下都是以 Ubuntu 11.10 為操作環境

主要目標為使用 rvm (Ruby Version Manager)來管理 ruby 版本並安裝 ruby 和 rails

先安裝一些必要套件 ( ex. openssl, zlib1g-dev ....etc. )

```
$sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev nodejs
```

安裝完基本套件後, 有兩種選擇:

1. 直接在系統上安裝 ruby ( sudo apt-get install ruby )
2. 使用 rvm 來管理 ruby 版本

基本上有 rvm 來管理 ruby 版本是比較方便的, 可以隨時切換不同的 ruby 版本

(不過在使用一些套件上會有一些問題需要排解, 本篇最底下有 trouble shooting)
  
## RVM

安裝:

```
$bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer) 
$echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"
```
Load RVM function:  `~/.bash_profile`

指令:  

>rvm info -> 查看 rvm 做的設定
>
>rvm list known -> 列出所有可安裝的套件
>
>rvm list -> 列出目前已安裝的套件
>
>rvm install "package_name" -> 安裝套件
>
>rvm remove "package_name" -> 移除套件
>
>rvm 1.9.2 --default -> 設定預設的 ruby 版本

假設系統上的 ruby 版本是 1.9.1, 而我用 rvm 安裝了 ruby1.9.3

那麼目前系統上的預設 ruby 版本是 1.9.3, 用 `ruby -v` 測試看看

如果要切換回系統的 ruby 版本, 則使用 `rvm system `

要切換回來的話,則:

`rvm 1.9.3` (直接輸入有安裝過的 ruby 版號即可)

or
		
`rvm use 1.9.3`

以上是 rvm 的一些簡單指令, 因為今天是要安裝 ruby on rails

所以先安裝 ruby 吧:

```
$rvm pkg install openssl (詳見最後的 trouble shooting)
$rvm install 1.9.3 --with-openssl-dir=$rvm_path/usr
```

## Gem

指令:

>gem list -> 列出目前已安裝的套件
>
>gem install "package_name" -> 安裝套件不同的 ruby 版本有不同的 gem

如果 rvm 裡有 1.9.1 和 1.9.3 兩個乾淨的 ruby 版本

目前使用的 ruby 版本是 1.9.3

則在 gem install rails 後

使用 gem list 則會看到 一大堆的套件

接著切換到 1.9.1 執行 gem list 後則會發現沒有任何東西

在安裝 gem 套件時可以加上一些參數

如: 不想要有說明文件 (通常這些東西都蠻肥大的) 就可以加上

`gem install "gem_name" --no-ri --no-rdoc`

以上是 gem 在 rvm 裡的一些概念, 因為今天是要安裝 ruby on rails

所以來安裝 rails 吧:

`gem install rails --no-ri --no-rdoc (不要安裝文件, 速度會比較快)`

如果發現用 gem 安裝套件時需要 root 權限, 表示目前使用的 ruby 版本是系統的版本


## Gemset
rvm 提供了方便的功能 gemset, 就把它看作 gem 的分類資料夾

如果說我想要在 ruby 1.9.2 底下安裝兩種 rails 版本, 又怕會衝突該怎麼辦?

就可以用 gemset 了

樹狀架構大概如下:

```
		rvm -> 1.9.1 -> gemset: rails-2.3.9
		                        rails-3.1.1*
		    -> 1.9.3 -> gemset: rails-2.3.9
		                        rails-3.1.1
```

可以解釋為: 在 rvm 的 ruby 1.9.1 底下有兩個 gemset, 分別是 rails-2.3.9 和 rails-3.1.1

指令:  

>rvm gemset list -> 查看目前建立的 gemset
>
>rvm 1.9.2@rails-3.1.1 -> 在 ruby 1.9.2 版本底下使用 rails-3.1.1 這個 gemset
>
>rvm gemset create "gemset_name" -> 建立 gemset
>
>rvm gemset use "gemset_name" -> 使用某個 gemset
>
>rvm gemset empty "gemset_name" -> 清空某個 gemset (只是清空)
>
>rvm gemset delete "gemset_name" -> 刪除某個 gemset以上就是關於 rvm 的二三事, 做到這邊, ruby 和 rails 套件應該就安裝完了

最後安裝資料庫的 adapter , 這裡是用 sqlite

`gem install sqlite3 --no-ri --no-rdoc`

接著就可以建立第一個 rails 專案:

>mkdir project
>
>cd project
>
>rails new demo
>
>rails server連上 localhost:3000 就是起始畫面了

## Trouble shooting:
### Error message1: openssl  LoadError

>SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (OpenSSL::SSL::SSLError)
>
>or
>
>Gem::RemoteFetcher::FetchError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B

Solution:

>rvm pkg install openssl
>
>rvm remove 1.9.3
>
>rvm install 1.9.3 --with-openssl-dir=$rvm_path/usr
>
>然後將 rvm 裡的 ssl certs 資料夾移除, 改成直接讀系統的 ssl certs
>
>rmdir $rvm_path/usr/ssl/certs
>
>ln -s /etc/ssl/certs $rvm_path/usr/ssl

### Error message2:  

>when excuting the command "gem install rails"
>
>ERROR:  Loading command: install (LoadError)
>
>cannot load such file -- zlib
>
>ERROR:  While executing gem ... (NameError)
>
>uninitialized constant Gem::Commands::InstallCommand

Solution:  

>rvm remove 1.9.3 (depends on your ruby version, if your system also has ruby, remove it, too.)
>
>sudo apt-get install zlib1g-dev先將 rvm 的 ruby 和 system 的 ruby 移除
>
>然後安裝 zlib1g-dev
>
>安裝 system 和 rvm 的 ruby 就可以了
>
>Remove system's ruby, and all ruby versions in rvm
>
>Install zlib1g-dev, then install ruby in system and rvm.
  
### Error message3:  

>ExecJS::RuntimeUnavailable

Solution: 

`sudo apt-get install nodejs`

Reference:

[RVM - Ruby enVironment (Version) Manager][1]

[1]: http://www.openfoundry.org/tw/tech-column/8513-rvm-ruby-environment-version-manager
