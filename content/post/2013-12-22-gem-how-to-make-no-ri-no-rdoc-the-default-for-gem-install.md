---
title: '[Gem] How to make --no-ri --no-rdoc the default for gem install?'
description: "設定 ~/.gemrc 讓每次 gem install 預設加上 --no-ri --no-rdoc，省去手動輸入旗標的麻煩，加速安裝過程。"
date: 2013-12-22
categories: ['Ruby']
---


In home dir, create a file: .gemrc (on linux, maybe on mac)

```
echo "gem: --no-ri --no-rdoc" >> ~/.gemrc
```
