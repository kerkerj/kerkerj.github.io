---
title: '[Ubuntu] 12.04 install oracle java'
description: "Ubuntu 12.04 安裝Oracle Java的完整指令，清除舊版後加入 webupd8 PPA，再透過 apt-get 安裝 oracle-java7-installer。"
date: 2013-11-26
categories: ['Java', 'Linux']
---


```
sudo rm /var/lib/dpkg/info/oracle-java7-installer*
sudo apt-get purge oracle-java7-installer*
sudo rm /etc/apt/sources.list.d/*java*
sudo apt-get update
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
```
