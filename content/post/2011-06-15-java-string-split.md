---
title: '[Java] String split 字串分割'
description: "本文透過簡潔的 Java 程式碼範例，示範如何使用 `String.split()` 方法依據指定分隔符號來分割字串，並逐一列印分割後的陣列元素。"
date: 2011-06-15
categories: ['Java']
---


```java
	String temp = "data1, data2, data3, data4"; //欲分割的字串
  String data[] = temp.split(","); //分割後存進字串陣列
  
  //印出每一個陣列元素
  for(String result:data){
  	System.out.println(result);
  }
```  
	  
  
  
  


