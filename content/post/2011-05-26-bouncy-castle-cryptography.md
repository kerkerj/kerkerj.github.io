---
title: '[Java] Bouncy Castle Cryptography'
description: "Java 密碼學作業實作紀錄，使用 Bouncy Castle 函式庫完成 SHA512 摘要、RSA 非對稱加密及 AES 對稱加密，處理文字、圖片與聲音檔案。"
date: 2011-05-26
slug: bouncy-castle-cryptography
tags: ['java']
---


這是為了 ECT 作業所作的筆記
[Bouncy Castle Cryptography][1]

這次作業用到了密碼學的技術，助教給了這個網站的 library 讓大家方便實作  

作業內容是：可讀取文字, 圖片, 聲音  
先初始化轉成 byte 後，利用 SHA512 進行訊息摘要，  
再對檔案做 RSA 加密, 解密,接著是 AES 加密, 解密,   
最後使用解密後的內容作還原的動作,  
若 input 的檔案與 output 的檔案內容一樣表示成功。  

以下是這次作業會用到的 class   
```
* SHA512Digest 產生訊息摘要MD  
* AESEngine (對稱式加密)  
* RSAEngine (非對稱式加密)  
* RSAKeyParameters ，用來產生RSA的公鑰、私鑰  
* KeyParameter  
* BigInteger  
```

p.s. 以下是在寫作業時遇到的問題解決網址  
[其實用到了什麼class也是google後從學長的部落格看來的XD][2]  

SHA512: [SHA512 ouput 問題][3]  
發現除了 SHA 的 out 外  
其他的加解密產生 output 值要印出時都可以用到：  
用： `String.format("%0128x", new BigInteger(1, byteData));`  
來取代 `Hex.encode(byteData);`  

RSA:  
[JavaWorld - 在RSA 解密時的問題 ][4]  
[使用Java進行RSA加解密][5]  
[RSA using BouncyCastle][6] (有問題 不過解決了 --> JavaClassCastException)   

AES:  
[用Bouncy Castle实现AES-128-CBC加密解密 (殘體字不喜勿入)][7]  
[How to encrypt / decrypt with AES from Bouncy Castle API in J2ME applications][8]  

最後的檔案輸出：  
[良葛格的學習筆記][9]  (最後寫輸出時才想到有這個網站，因此也修改了輸入檔案時的做法，一氣呵成~~ )  

若有一直google的話，會發現有許多做法  
我認為以下做法最省事：(省略了很多作法，自行google)  
```java
Cipher out = Cipher.getInstance("演算法的名字 (ex. AES)", "BC");
out.init(Cipher.DECRYPT_MODE (或是ENCRYPT_MODE), key, new IvParameterSpec(AESiv));
outputByteArray = out.doFinal( inputByteArray);   (通常因為長度很長所以會用迴圈去做分段讀取的動作)
```

2011/06/02  Updated:  
單純加解密：  
RSA - [http://www.javamex.com/tutorials/cryptography/rsa_encryption.shtml][10]  
AES - [http://java.sun.com/developer/technicalArticles/Security/AES/AES_v1.html][11]  
超級有用 - [http://introcs.cs.princeton.edu/java/78crypto/RSA.java.html][12]  
  
  

[1]: http://www.bouncycastle.org/docs/docs1.6/index.html
[2]: http://danielroc0108.blogspot.com/2008/06/legion-of-bouncy-castle.html
[3]: http://stackoverflow.com/questions/2208374/how-can-i-create-an-sha512-digest-string-in-java-using-bouncy-castle
[4]: http://www.javaworld.com.tw/jute/post/view?bid=29&id=90179&tpg=8&ppg=1&sty=1&age=0
[5]: http://borispong.pixnet.net/blog/post/23242319
[6]: http://ox.no/posts/rsa-using-bouncycastle
[7]: http://albertsong.iteye.com/blog/196273
[8]: http://www.itcsolutions.eu/2010/12/28/how-to-encrypt-decrypt-with-aes-from-bouncy-castle-api-in-j2me-applications/
[9]: http://caterpillar.onlyfun.net/Gossip/JavaGossip-V2/ByteArrayInOutStream.htm
[10]: http://www.javamex.com/tutorials/cryptography/rsa_encryption.shtml
[11]: http://java.sun.com/developer/technicalArticles/Security/AES/AES_v1.html
[12]: http://introcs.cs.princeton.edu/java/78crypto/RSA.java.html
