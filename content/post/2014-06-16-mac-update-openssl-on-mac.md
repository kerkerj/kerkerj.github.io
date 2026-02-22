---
title: '[Mac] update openssl on mac'
description: "Mac 使用 Homebrew 更新 OpenSSL 的步驟，若版本仍顯示舊版，需手動刪除 /usr/bin/openssl 並建立指向新版的 symlink。"
date: 2014-06-16
categories: ['macOS', 'DevOps']
---


First of all, you need 'brew' ([http://brew.sh/](http://brew.sh/))

```
$ brew update
$ brew install openssl
$ brew link --force openssl
 
$openssl version -a
```
If it's still the old version, you shoud:

```
$ sudo rm /usr/bin/openssl 	#remove the old binary 
$ sudo ln -s /usr/local/Cellar/openssl/1.0.1h/bin/openssl /usr/local/bin
(1.0.1h -> the latest version)
``` 

ref: [http://apple.stackexchange.com/questions/126830/how-to-upgrade-openssl-in-os-x](http://apple.stackexchange.com/questions/126830/how-to-upgrade-openssl-in-os-x)
