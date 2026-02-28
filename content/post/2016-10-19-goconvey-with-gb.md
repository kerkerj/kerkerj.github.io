---
date: "2016-10-19T00:01:04+08:00"
slug: goconvey-with-gb
draft: false
tags: ['golang', 'gb', 'goconvey']
title: "Use goconvey in projects maganged by gb"
description: "在以 gb 管理的 Golang 專案中啟動 goconvey web UI 的方法，透過正確設定 GOPATH 環境變數後執行 goconvey 指令。"
---


這是前陣子在工作上用到的

有個 golang 的專案，使用了 [gb](https://getgb.io/) 來管理第三方套件

當時想套 [goconvey](http://goconvey.co/) 進來

但是一直搞不定執行的方式

後來在 [github goconvey issue](https://github.com/smartystreets/goconvey/issues/345#issuecomment-150949311) 裡找到了方式

```
$ cd /to/your/gb_projects
$ PROJECT_DIR=`pwd`
$ GOPATH="$PROJECT_DIR/vendor:$PROJECT_DIR" goconvey -packages=1 -port 8899
```

這樣就會跑 web ui 出來摟

不想跑 web ui 就執行 `gb test -v` 就可以了...上面這樣只是單純想跑 web ui 而已 XD