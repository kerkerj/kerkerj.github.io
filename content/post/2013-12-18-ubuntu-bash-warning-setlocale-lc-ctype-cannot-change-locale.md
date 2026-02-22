---
title: '[Ubuntu] -bash: warning: setlocale: LC_CTYPE: cannot change locale'
description: "Ubuntu bash 出現 'setlocale: LC_CTYPE: cannot change locale' 警告的修復指令：執行 locale-gen zh_TW.UTF-8 並 update-locale 後重新載入設定檔。"
date: 2013-12-18
categories: ['Linux']
---


```
$ sudo locale-gen zh_TW.UTF-8
$ sudo update-locale LANG=zh_TW.UTF-8
```
reload your bash profile
