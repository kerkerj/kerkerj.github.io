---
title: '[Android] 遠端主機已強制關閉一個現存的連線'
description: "Android 開發實體機連線 PC 頻繁出現『遠端主機已強制關閉連線』錯誤的解法，下載修補版 adb.exe 替換 platform-tools 中的舊版即可。"
date: 2012-04-11
slug: android-remote-shutdown
tags: ['android']
---


不知道大家在寫 Android 程式時使用實體機接 PC 時會不會很常出現這個錯誤

我個人還蠻常出現的，在測試時是非常緩慢的

Google 了一下，找到了強者自己寫的 adb.exe

檔案在討論串裡：[http://code.google.com/p/android/issues/detail?id=12141][1]  

	Nov 28, 2011  
	New patched version of adb (1.0.29)   
	Patch (diff) the same as early  
	adb.exe   
	478 KB Download  

Download: [adb.exe][2]

下載後，到 Android 的 SDK 資料夾，進 platform-tool 資料夾，

將原本的 adb.exe 做備份，再將新的複製到原本的 exe 檔所在的地方，

重新啟動 adb 就可以了 (進 DDMS 重新啟動，或者重新啟動 Eclipse 也行)

[1]: http://code.google.com/p/android/issues/detail?id=12141
[2]: http://android.googlecode.com/issues/attachment?aid=121410050000&name=adb.exe&token=O8Q8Mzj8XLo6-cOEE6a2H8SG4Bg%3A1334136508184
