---
title: '[Java] Hex String & Byte Array 轉換'
description: "Java Hex String 與 Byte Array 互轉的程式碼片段，使用 BigInteger 類別實作 toHexString 和 fromHexString 兩個轉換方法。"
date: 2011-06-01
slug: java-hex-string-byte-array
tags: ['java']
---


```java
public String toHexString(byte[] in){
	BigInteger temp = new BigInteger(in);
  return temp.toString(16);
}

public byte[] fromHexString(String in){
	BigInteger temp = new BigInteger(in, 16);
  return temp.toByteArray();
}  
```

文章參考[自此][1]  
  
  


[1]: http://ranger1976.blogspot.com/2008/07/hex-string-to-byte-array-and-reverse.html
