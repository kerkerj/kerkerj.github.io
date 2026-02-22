---
title: '[Java] Hex String & Byte Array 轉換'
description: "本文提供 Java 程式碼範例，示範如何利用 `BigInteger` 類別，高效地將十六進位字串轉換為位元組陣列，反之亦然。"
date: 2011-06-01
categories: ['Java']
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
