---
title: '[Ubuntu] update OpenSSL'
description: "Ubuntu 手動更新 OpenSSL 的兩種方法：簡便的 curl+make 一行指令，以及含 GPG 簽章驗證的完整安全 bash script，適用於 Heartbleed 等漏洞修補。"
date: 2014-04-16
categories: ['Linux', 'DevOps']
---


Easy way:

* Below the single command line to compiling and install the last openssl version.

```
$ curl https://www.openssl.org/source/openssl-1.0.1h.tar.gz | tar xz && cd openssl-1.0.1h && sudo ./config && sudo make && sudo make install
```
* Replace old openssl binary file by the new one via a symlink.

```
$ sudo ln -sf /usr/local/ssl/bin/openssl `which openssl`
```

how to check version:

```
$ openssl version -b
built on: Mon Apr  7 20:33:29 UTC 2014
$ openssl version -a
OpenSSL 1.0.1 14 Mar 2012
built on: Mon Apr  7 20:33:29 UTC 2014
platform: debian-amd64
options:  bn(64,64) rc4(16x,int) des(idx,cisc,16,int) blowfish(idx)
compiler: ..........
OPENSSLDIR: "/usr/lib/ssl"
```
from: [http://superuser.com/questions/740930/apt-get-upgrade-openssl-wont-bring-ubuntu-12-04-to-latest-version](http://superuser.com/questions/740930/apt-get-upgrade-openssl-wont-bring-ubuntu-12-04-to-latest-version)

Hard way:

```
#!/bin/bash

###
# Need to upgrade an Ubuntu 13.04 server to use OpenSSL 1.0.1g?
# Read and execute this script :D
###
# License: WTFPL, GPLv3, MIT, whatever; just patch your shit
# http://askubuntu.com/questions/444702/how-to-patch-cve-2014-0160-in-openssl
###

if [[ $EUID -ne 0 ]]; then
	echo "This script must be run as root" 1>&2
	exit 1
fi
wget https://www.openssl.org/source/openssl-1.0.1g.tar.gz
wget https://www.openssl.org/source/openssl-1.0.1g.tar.gz.asc

gpg --recv-key 0xD3577507FA40E9E2
# Dr Stephen Henson
# IMPORTANT! Manually verify that this is the correct key ID:
# http://pgp.mit.edu:11371/pks/lookup?op=vindex&search=0xD3577507FA40E9E2
# https://www.openssl.org/about/

gpg --verify openssl-1.0.1g.tar.gz.asc openssl-1.0.1g.tar.gz

if [[ $? -eq 0 ]]; then
	tar xzvf openssl-1.0.1g.tar.gz
	cd openssl-1.0.1g && sudo ./config && sudo make && sudo make install
	# To link the old openssl library to a new version
	sudo ln -sf /usr/local/ssl/bin/openssl `which openssl`
	echo
	echo "DONE!"
fi

# eof
```
