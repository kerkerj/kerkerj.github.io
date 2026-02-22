---
title: '[Android] 程式開發雜記'
description: "Android 開發雜記，彙整各 Java 套件功能速查、Empty/Background/Foreground 等處理程序種類，以及 Activity、Service 等核心組件介紹。"
date: 2011-05-06
categories: ['Android']
---


最近在寫和 Android 程式，因此把一些重點筆記下來：

## Android 中各種 JAVA 包的功能描述
(ref: [http://huenlil.pixnet.net/blog/post/24346240][1])

在 Android 的應用程序開發中，通常使用的是 JAVA 語言，除了需要熟悉 JAVA 語言的基礎知識之外，還需要瞭解 Android 提供的擴展的 JAVA 功能。
在 Android 中，各種包寫成 android.*的方式，重要包的描述如下所示：

```
* android.app        ：提供高層的程序模型、提供基本的運行環境
* android.content    ：包含各種的對設備上的數據進行訪問和發佈的類
* android.database   ：通過內容提供者瀏覽和操作數據庫
* android.graphics   ：底層的圖形庫，包含畫布，顏色過濾，點，矩形，可以將他們直接繪製到屏幕上
* android.location   ：定位和相關服務的類
* android.media      ：提供一些類管理多種音頻、視頻的媒體接口
* android.net        ：提供幫助網絡訪問的類，超過通常的java.net.* 接口
* android.os         ：提供了系統服務、消息傳輸、IPC機制
* android.opengl     ：提供OpenGL的工具
* android.provider   ：提供類訪問Android的內容提供者
* android.telephony  ：提供與撥打電話相關的API交互
* android.view       ：提供基礎的用戶界面接口框架
* android.util       ：涉及工具性的方法，例如時間日期的操作
* android.webkit     ：默認瀏覽器操作接口
* android.widget     ：包含各種UI元素（大部分是可見的）在應用程序的屏幕中使用
```

## Empty Process, Background Process, Service Process, Visible Process, Foreground Process 初探
(ref: [[Android 教學課程] Empty Process , Background Process , Service Process , Visible Process , Foreground Process 初探][2] )

```
* 關於 Android 的應用程式組件 ( Application Components ) 的補充
* 處理程序 ( Process ) 與執行緒 ( Thread )
* 空白處理程序 ( Empty Process )
* 背景處理程序 ( Background Process )
* Service 處理程序 ( Service process )
* 可視處理程序 ( Visible Process )
* 前景處理程序 ( Foreground process ) 
```

## Activity , Fragment , Service , Broadcast receiver , Content provider 與 Intent 初探
( ref: [[Android 教學課程] Activity , Fragment , Service , Broadcast receiver , Content provider 與 Intent 初探][3] )

```
* 什麼是 Activity？	Activity ：它負責前端使用者介面處理。
* 什麼是 Fragment？	Fragment ：它在一個具分割畫面( Multi-Pane )的 Activity 應用程式組件之中，負責分割畫面的部份區段或部份行為。
* 什麼是 Service?	Service ：它負責後端程式運算。
* 什麼是 Content provider？	Content provider ：它負責應用程式之間資料共享的任務。
* 什麼是 Broadcast receivers？	Broadcast receivers ：此應用程式組件的中文譯名，多半會翻成廣播接收者或者廣播接收器。
```

## 手機截圖：
(ref:[【教學】Android 手機截圖：一點都不難][4])

若有裝Android SDK 的話 (一般開發者應該都會裝吧XD)，直接開 Android SDK 的目錄 /tools/ddms.bat，執行此批次檔就可以囉 (使用時不要將該dos畫面關閉), 此時手機必須連接上網路，並設定USB偵錯，
要截圖時只要到 device/capture screen 即可

[1]: http://huenlil.pixnet.net/blog/post/24346240
[2]: http://www.gururu.tw/Android-Process-Thread.html
[3]: http://www.gururu.tw/Android-Activity-Fragment-Service-Broadcast-receiver-Content-provider-Intent.html
[4]: http://www.eprice.com.tw/mobile/talk/124/4483653/
