---
title: '[Codeigniter] use markdown as library'
description: "How to integrate Parsedown Markdown parser into CodeIgniter as a library. Download Parsedown, place it in the Library folder, configure autoload.php, and render Markdown to HTML in your Views."
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

