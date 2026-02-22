---
title: "Go pprof"
date: 2018-08-21T16:12:37+08:00
categories: ['golang']
tags: ['golang', 'pprof', 'go-torch', 'trace']
keywords: ['golang','pprof','kerkerj']
clearReading: true
comments: true
showTags: true
showPagination: true
showSocial: true
showDate: true
showMeta: true
showActions: true
#disqusIdentifier: ""
#thumbnailImage: image-1.png
#thumbnailImagePosition: bottom
#autoThumbnailImage: yes
#metaAlignment: center
#coverImage: image-2.png
#coverCaption: "A beautiful sunrise"
#coverMeta: out
#coverSize: full
#coverImage: image-2.png
#gallery:
#    - image-3.jpg "New York"
#    - image-4.png "Paris"
#    - http://i.imgur.com/o9r19kD.jpg "Dubai"
#    - https://example.com/orignal.jpg https://example.com/thumbnail.jpg "Sidney"
---



簡單筆記一下會用到的 pprof 相關指令

<!--more-->



## Prepare

使用 `net/http/pprof` 提供的 routes 來 profiling

```go
import _ "net/http/pprof"
```

​用 `gin` 的話可以直接用這個 lib https://github.com/gin-contrib/pprof

可以的話盡量不要讓 pprof routes 暴露在外面，看是聽另一個 port 或是限制 routing，都只給內網存取。



## Profiling

幾個常見的 profile 指令列於下方

### Memory profiling - heap profile

```
$ go tool pprof http://ip:port/debug/pprof/heap
```

### CPU profiling - 30-second CPU profile

```
$ go tool pprof http://ip:port/debug/pprof/profile
(pprof) top10
(pprof) web
```

或是直接存取 http://ip:port/debug/pprof/profile，預設 30 秒後會提供檔案下載。

### Collect a 5-second execution trace

```
$ wget http://ip:port/debug/pprof/trace?seconds=5
```

### go-torch

使用 `go-torch` 來產生火焰圖，但如果是 go `1.10` 之後已經內建可以看火焰圖，就不需要這個了

```
$ go-torch --url=http://ip:port/debug/ --suffix=/debug/pprof/profile --seconds 60 --file torch.svg
```

### go execution tracer

```
$ curl http://ip:port/debug/pprof/trace?seconds=30 > trace.out
$ go tool trace trace.out
```

如果是要找出跑得比較慢的函式，或是找出大部分的 CPU 時間花在什麼地方，用 `pprof`；`execution tracer` 則適合拿來追蹤函式的流程，或是分析 race condition 之類資源搶奪的問題。

補充一下之前 Dave Cheney 分享的 workshop

https://github.com/davecheney/understanding-the-execution-tracer