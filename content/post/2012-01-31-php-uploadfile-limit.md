---
title: '[PHP] 上傳檔案的限制'
description: "本文列出 `php.ini` 中關於 PHP 腳本執行時間、輸入處理時間、記憶體用量、POST 資料大小、單檔上傳容量及 Socket/MySQL 連線逾時等關鍵限制設定。"
date: 2012-01-31
categories: ['PHP']
---


php.ini

1. max_execution_time - Script執行時間上限(單位：秒)

2. max_input_time - Script處理資料時間上限(單位：秒)

3. memory_limit - 系統記憶體 (要比4,5大

4. post_max_size - 表單的POST發送量

5. upload_max_filesize - 單次上傳檔案容量

6. default_socket_timeout - Socket無回應斷線時間(單位：秒)

7. mysql.connect_timeout - 無回應斷線時間(單位：秒；-1代表不斷線一直等)
  



