---
title: '[Mac] update openssl on mac'
description: "First of all, you need 'brew' (http://brew.sh/) $ brew update $ brew install openssl $ brew link --force openssl $openssl version -a If it's still..."
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
