---
title: '[Ubuntu] -bash: warning: setlocale: LC_CTYPE: cannot change locale'
description: "本文針對 Ubuntu 上 `bash: warning: setlocale: LC_CTYPE: cannot change locale` 錯誤，提供快速解決方案：透過 `locale-gen` 生成 `zh_TW.UTF-8` 語系，並更新系統預設語系設定。"
date: 2013-12-18
categories: ['Linux']
---


```
$ sudo locale-gen zh_TW.UTF-8
$ sudo update-locale LANG=zh_TW.UTF-8
```
reload your bash profile
