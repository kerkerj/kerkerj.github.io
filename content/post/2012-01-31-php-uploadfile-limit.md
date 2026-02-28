---
title: '[PHP] 上傳檔案的限制'
description: "php.ini 中影響檔案上傳的七項關鍵設定說明，包含 max_execution_time、memory_limit、post_max_size、upload_max_filesize 等限制參數。"
date: 2012-01-31
slug: php-uploadfile-limit
tags: ['php']
---


php.ini

1. max_execution_time - Script 執行時間上限(單位：秒)

2. max_input_time - Script 處理資料時間上限(單位：秒)

3. memory_limit - 系統記憶體 (要比 4,5 大

4. post_max_size - 表單的 POST 發送量

5. upload_max_filesize - 單次上傳檔案容量

6. default_socket_timeout - Socket 無回應斷線時間(單位：秒)

7. mysql.connect_timeout - 無回應斷線時間(單位：秒；-1 代表不斷線一直等)
  



