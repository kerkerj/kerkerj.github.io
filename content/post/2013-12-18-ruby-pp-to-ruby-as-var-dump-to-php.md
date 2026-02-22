---
title: '[Ruby] PP to Ruby as Var_dump to PHP '
description: "本文為 Ruby 開發者提供實用技巧，說明 `pp` (pretty print) 方法在引入 `pp` 函式庫後，能像 PHP 的 `var_dump` 一樣，方便地輸出變數內容以供檢查。"
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
