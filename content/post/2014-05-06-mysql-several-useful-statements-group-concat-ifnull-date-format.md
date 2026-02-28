---
title: '[MySQL] Several useful statements (GROUP_CONCAT, ifnull, DATE_FORMAT)'
description: "MySQL 三個實用語句：GROUP_CONCAT 將多行結果合併成一個欄位、DATE_FORMAT 格式化日期顯示，以及 IFNULL 為空值欄位設定預設值。"
date: 2014-05-06
slug: mysql-several-useful-statements-group-concat-ifnull-date-format
tags: ['mysql']
---


1. 將 query 出來的多筆記錄結合成一個欄位  
例如原本的 query 結果為：  
`SELECT 'fruits' FROM 'fruits_table'`  
apple  
banana  
kiwi  
`SELECT group_concat('fruits' separator ',') FROM 'fruits_table'`  
result: apple,banana,kiwi    
  
2. 改變 datetime 欄位的顯示結果  
`SELECT DATE_FORMAT( 'created_at', '%Y/%m/%d %H:%i') AS 'created_at' FROM 'message_table'`  
result: 2014/05/06 18:20   

3. 若某個欄位的值為空，則給予預設值  
`SELECT ifnull('is_success', 0) AS 'is_success' FROM .... `  
