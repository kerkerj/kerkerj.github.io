---
title: '[Ubuntu] 將 Big5 文字檔轉成 UTF-8 '
description: "本文示範在 Ubuntu 環境下，如何使用 `iconv` 指令將 Big5 編碼的文字檔轉換為 UTF-8 編碼，並提供將結果輸出至新檔或直接覆蓋原檔的範例。"
date: 2011-08-15
categories: ['Linux']
---


```
$ iconv -f big5 -t utf-8 "要轉的big5檔案路徑" -o  "轉好後要輸出的檔案名"  
$ iconv -f big5 -t utf-8 /home/test/bigtext.txt -o  /home/test/utf8text.txt  
```

也可以輸入和輸出是同一個檔案  

```
$ iconv -f big5 -t utf-8 /home/test/text.txt -o  /home/test/text.txt  
```
  
  



