---
title: '[MySQL] Several useful statements (GROUP_CONCAT, ifnull, DATE_FORMAT)'
description: "1. 將 query 出來的多筆記錄結合成一個欄位 例如原本的 query 結果為： SELECT 'fruits' FROM 'fruitstable' apple banana kiwi SELECT groupconcat('fruits' separator ',') FROM..."
date: 2014-05-06
categories: ['MySQL']
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
