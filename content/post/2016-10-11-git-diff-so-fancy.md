---
date: 2016-10-11T11:10:52+08:00
draft: false
title: "[tool] diff-so-fancy"
description: "diff-so-fancy 工具讓 git diff 輸出更漂亮易讀，透過 Homebrew 安裝後設定 core.pager 並加上高亮顏色配置。"
categories: ['Git', '工具']
tags: ["git", "tool"]
---


現在想到什麼都來 PO 一下 XD

https://github.com/so-fancy/diff-so-fancy

拿來幫你把 `git diff ` 變漂亮的東東

````ruby
// install
$ brew install diff-so-fancy

// Setup
$ git config --global core.pager "diff-so-fancy | less --tabs=4 -RFX"

// make it more fancier
$ git config --global color.diff-highlight.oldNormal "red bold"
$ git config --global color.diff-highlight.oldHighlight "red bold 52"
$ git config --global color.diff-highlight.newNormal "green bold"
$ git config --global color.diff-highlight.newHighlight "green bold 22"
````

