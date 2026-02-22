---
title: '[Codeigniter] remove index.php & 圖片無法讀取問題'
description: "本文解決 CodeIgniter 框架中移除 URL 的 `index.php` 及圖片無法讀取的問題，透過修改 `.htaccess` rewrite 規則與 `config.php` 設定，確保網站路由正常運作。"
date: 2011-12-20
categories: ['PHP']
---


首先，是先簡單說明使用 Codeigniter framework 時移除 URL 中的 index.php  
在 Codeigniter 的根目錄新增一 .htaccess 檔案  
並放入以下內容  

```
RewriteEngine on  
RewriteCond $1 !^(index\.php|js|robots\.txt|css)  
RewriteRule ^(.*)$ index.php/$1 [L]  
```

另外還要修改 Codeigniter 的 config.php 設定  

`$config['index_page'] = ''";`

不過上面的 rewrite rule 有些問題，  
情境是這樣的：  
我在 Codeigniter 根目錄新增一資料夾為 uploads ，  
放置上傳的圖片與影片，  
但是因為 rewrite rule routing 的關係沒辦法讀取到圖片，  
因此我將 .htaccess 檔修改如下：  

```
RewriteEngine on  
RewriteCond $1 !^(index\.php|images|css|js|robots\.txt|favicon\.ico)  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d  
RewriteRule ^(.*)$ ./index.php?/$1 [L,QSA]  
```

主要是在三四行的 `%{REQUEST_FILENAME}`  
這樣修改完就解決問題了!  


ref: [CodeIgniter 如何去掉 URL 中的 index.php][1]

[1]: http://milk4candy.wordpress.com/2010/12/31/codeigniter%E5%A6%82%E4%BD%95%E5%8E%BB%E6%8E%89-url-%E4%B8%AD%E7%9A%84-index-php/
