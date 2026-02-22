---
title: '[Ruby] PP to Ruby as Var_dump to PHP '
description: "Ruby 中 pp（pretty print）的使用方法，require 'pp' 後即可像PHP的 var_dump 一樣輸出變數詳細內容，方便除錯。"
date: 2013-12-18
categories: 
---


```ruby
require 'pp'
pp(ENV)
```

>Usage:
>pp(any_variable)

just like `var_dump` in `php`
