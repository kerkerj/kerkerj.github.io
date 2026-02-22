---
title: '[Ubuntu] 11.04 Dropbox 啟動問題'
description: "Ubuntu 11.04 安裝Dropbox後設定頁面遲遲不出現的修復方法，下載並執行 fixdropbox 腳本即可解決 AppIndicator 問題。"
date: 2011-05-02
categories: ['Linux', '其他']
---


我從 Dropbox 上下載 for Ubuntu 的套件，安裝完後，設定的頁面卻遲遲不出來，這真是非常的詭異，所以就查了解決方法。   
[http://www.webupd8.org/2011/03/get-dropbox-appindicator-to-work-in.html#more][1]  
  
1. 一樣也是先去下載  
2. 然後執行以下指令（一行一行)  

```
$cd  
$wget http://webupd8.googlecode.com/files/fixdropbox  
$chmod +x fixdropbox  
$./fixdropbox  
```

應該就可以了，如果要多作設定再：`gedit ~/.dropbox.sh`  

好像有什麼秒數的東西設定吧~~   

[1]: http://www.webupd8.org/2011/03/get-dropbox-appindicator-to-work-in.html#more
