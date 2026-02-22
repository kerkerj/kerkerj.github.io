---
categories: ['Docker', 'DevOps']
date: 2016-10-18T23:36:40+08:00
draft: false
tags: ["docker","haproxy","docker-compose"]
title: "docker HA proxy"
description: "使用 dockercloud/haproxy 搭配 docker-compose 架設 web app × 3 + Redis + HAProxy 的負載均衡測試環境，並用 docker stats 觀察 container metrics。"
---


不知道標題該下啥...

前陣子因為工作需要，需要測試一個 web app 分布在多台機器下的狀況

想說使用 docker 來做這件事，但又懶得弄 nginx 的設定

稍微查了一下發現有 [dockercloud/haproxy](https://github.com/docker/dockercloud-haproxy)

我使用的情境是 web app * 3 + ha * 1 + redis * 1 

web 使用了兩個 port 7788, 7789

但是不想讓 ha 把流量導去 7788，所以可以設定 EXCLUDE_PORTS

<script src="https://gist.github.com/kerkerj/32333fb07cb34541d9134735d531bcb3.js"></script>

如此一來，在 `docker-compse up` 後，就可以透過 `http://192.168.99.100:5566/` 來連上了

並且可以透過 `docker stats $(docker ps -q)` 這個指令來觀察正在執行中的 containers 的基本 metrics~

快速簡單!
