---
title: '[Ubuntu] 12.04 install oracle java'
description: "本文提供在 Ubuntu 12.04 上安裝 Oracle Java 的 shell 指令，包含清除舊有安裝、新增 PPA 儲存庫、更新套件清單，以及最終安裝 `oracle-java7-installer` 的步驟。"
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
