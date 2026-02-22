---
title: "kubectx"
date: 2019-05-20T22:23:45+08:00
categories: ['k8s', 'command']
tags: ['k8s']
keywords: ['tech']
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

-

<!--more-->

最近在學習 k8s（lag 好久）

好用工具：https://github.com/ahmetb/kubectx

自己用到的情境是，會有多個 kube config，因此會先用 `KUBECONFIG` 這個環境變數預先定義好，例如：

```
# ~/.zshrc
export KUBECONFIG=~/.kube/config-prod:~/.kube/config-staging

# or merge into one file:
# KUBECONFIG=~/.kube/this-config:~/.kube/other-config kubectl config view --flatten > ~/.kube/config
```

使用 `kubectx` 後就可以多選（使用 [fzf](https://github.com/junegunn/fzf) 更方便 :D）





