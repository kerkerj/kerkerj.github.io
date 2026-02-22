---
title: '[Codeigniter] Disallowed Key Characters'
description: "本文針對 CodeIgniter 框架中「Disallowed Key Characters」錯誤提供解決方案，特別指出表單名稱使用中文字元可能導致此問題，並建議避免使用非標準字元來修正。"
date: 2012-01-31
categories: ['PHP']
---


Codeigniter 顯示 Disallowed Key Characters  

我的情況是：  

表單(form)名稱型態不符合格式。  

不得使用中文。  

`＜input name="姓名" type="checkbox" value="1" /＞`  

改掉就好了  

ref: [Codeigniter 顯示 Disallowed Key Characters][1]  

[1]: http://clonn.blogspot.com/2010/12/codeigniter-disallowed-key-characters.html
