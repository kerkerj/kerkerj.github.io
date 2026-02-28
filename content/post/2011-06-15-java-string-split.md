---
title: '[Java] String split 字串分割'
description: "Java String.split() 字串分割的程式碼示範，以逗號分隔字串並存入陣列，再用 for-each 逐一列印每個元素。"
date: 2011-06-15
slug: java-string-split
tags: ['java']
---


```java
	String temp = "data1, data2, data3, data4"; //欲分割的字串
  String data[] = temp.split(","); //分割後存進字串陣列
  
  //印出每一個陣列元素
  for(String result:data){
  	System.out.println(result);
  }
```  
	  
  
  
  


