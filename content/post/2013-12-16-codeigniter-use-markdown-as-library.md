---
title: '[Codeigniter] use markdown as library'
description: "將 Parsedown Markdown 解析器整合進 CodeIgniter 的步驟：下載後放入 Library 資料夾並設定 autoload.php，即可在 View 中直接解析 Markdown 為 HTML。"
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

