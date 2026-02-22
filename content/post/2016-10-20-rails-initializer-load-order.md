---
categories: ['Rails']
date: 2016-10-20T10:39:30+08:00
draft: false
tags: ["rails","initializer"]
title: Rails initializer 的載入順序
description: "如果 rails app 裡的 initializer 有載入順序的需求的話 可以照著 Ruby On Rails Guide 這篇來設定 > If you have any ordering dependency in your initializers, you can control the..."
---


如果 rails app 裡的 initializer 有載入順序的需求的話

可以照著 [Ruby On Rails Guide](http://guides.rubyonrails.org/configuring.html#using-initializer-files) 這篇來設定

> If you have any ordering dependency in your initializers, you can control the load order through naming. Initializer files are loaded in alphabetical order by their path. For example, `01_critical.rb` will be loaded before `02_normal.rb`.

檔名加個數字前綴，rails 就會以數字順序來依序載入~