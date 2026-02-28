---
title: "Raspberry Pi2 remote webcam"
description: "Raspberry Pi 2 安裝 motion 作為遠端 webcam 串流伺服器，設定 motion.conf 開放串流存取，從瀏覽器遠端觀看攝影機即時影像。"
date: 2015-04-22
slug: raspberry-pi2-remote-webcam
tags: ['raspberry-pi']
---


如題~

先假設 pi2 本身的 IP 是 `192.168.1.200`

首先先進去 server 來更新一下~

```shell
$ sudo apt-get update && sudo apt-get upgrade
```

再來就裝 [motion](http://www.lavrsen.dk/foswiki/bin/view/Motion/WebHome)

他其實是一個 motion detector，不過也可以拿來當作 web cam 用的 streaming server XD

```shell
$ sudo apt-get install motion
```

編輯設定檔 `/etc/motion/motion.conf`

```shell
$ sudo vi /etc/motion/motion.conf
```

找到以下幾個值，並分別改成下面

```shell
daemon = ON
webcam_localhost = OFF
control_localhost = OFF
```

`webcam_localhost` 是 streaming 介面

`control_localhost ` 是設定介面 

如果要改預設 port，就找 `webcam_port`, or `control_port`

最後設定將 service 啟動

```shell
$ sudo vi /etc/default/motion
```

將 no 改成 yes

```shell
start_motion_daemon = yes
```

啟動 service

```shell
$ sudo service motion start
```

motion 這個 service 預設的 port 是 8080 / 8081 (可以在前述的設定檔更改)

開啟瀏覽器, 連入 `http://192.168.1.200:8080` or `http://192.168.1.200:8081`

這邊會有個小問題，就是連 8080 時會很正常

但是 8081 時如果是用 chrome，則怎麼都進不去

後來發現 [這篇](http://www.lavrsen.dk/foswiki/bin/view/Motion/SupportQuestion2013x09x01x104741)

他說 Chrome 不再支援 raw MJPEG streams 了

所以就是用其他瀏覽器開吧, 我是用 Safari :D

記得要和你的 pi2 在同一個 wifi 環境下~

剩下的還有許多設定可以玩～ 改天再來慢慢玩

