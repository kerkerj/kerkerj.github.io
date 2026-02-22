---
title: '[Ubuntu] 安裝設定 ssh'
description: "Ubuntu SSH 伺服器安裝與設定教學，涵蓋修改 sshd_config 變更 port、禁止 root 登入，以及用 hosts.allow/deny 精準控制允許連線的 IP。"
date: 2012-04-20
categories: ['Linux', 'DevOps']
---


Ubuntu 裝完是沒辦法透過別台機器 ssh 進這台機器  

只能夠 ssh 出去, 所以來安裝一下  

```
$sudo apt-get update  
$sudo apt-get install ssh  
```

裝完後修改一下設定檔 : `$sudo vi /etc/ssh/sshd_config`  

預設 port 為 22 可以改成別的 ( 可以到 `/etc/services` 看有沒有沒再用的 port 並指給它 ) (optional)  

`Port 22` 找到這行, 意思為允許 root 登入 (optional)  

`PermitRootLogin yes` 將 yes 改成 no, 不允許 root 登入.  

另外可以設定 `/etc/hosts.allow` 和 `/etc/hosts.deny` 更精準的限制連線 (optional)  

例如: 我只要 192.168.11.11 登入我這台機器  

則編輯 `/etc/hosts.allow` , 加入: `sshd:192.168.11.11 :allow`  

編輯 `/etc/hosts.deny` , 讓其他人都無法連入, 加入這行: `sshd:all:deny`  

全數編輯完畢後, 重新啟動 ssh 服務 `$sudo /etc/init.d/ssh restart`  
