---
title: '[AWS] ubuntu swap'
description: "Add swap for an instance $ sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024 $ sudo /sbin/mkswap /var/swap.1 $ sudo /sbin/swapon /var/swap."
date: 2013-12-26
categories: ['AWS', 'Linux']
---


Add swap for an instance

```
$ sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
$ sudo /sbin/mkswap /var/swap.1
$ sudo /sbin/swapon /var/swap.1
$ echo "/var/swap.1 swap swap defaults 0 0" >> /etc/fstab #將 swap 加入 開機啟動
```
在 Amazon EC2 micro plan，加入 swap 很容易 I/O 過量，
因此最好是需要時才開啟 swap，不用時關閉 swap 以免被收錢~

swap usage

```console
$ swapon -s   
$ free -k  
---
$ swapoff -a  
$ swapon  -a  
```

