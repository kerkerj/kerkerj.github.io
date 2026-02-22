---
title: '[Codeigniter] use markdown as library'
description: "本文簡要說明如何將 Parsedown Markdown 解析器整合至 CodeIgniter 專案，包含檔案放置、`autoload.php` 配置，以及在視圖中將 Markdown 文本轉換為 HTML 的使用範例。"
date: 2013-12-16
categories: 
---


[https://github.com/erusev/parsedown](https://github.com/erusev/parsedown) 

download the Parsedown.php
and put it into Library folder
then edit autoload.php

```
$autoload['libraries'] = array('Parsedown');
```

then in the view php for tests:

```
$text = 'Hello **Parsedown**!';
$result = Parsedown::instance()->parse($text);
echo $result;
```
No need to require or include the file.
If you want to load a file with path, you can use the function called file_get_contents:

```
file_get_contents('./markdown/test.md',true); (true: enable path)
```

