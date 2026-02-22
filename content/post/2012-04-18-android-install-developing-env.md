---
title: '[Android] 安裝開發環境'
description: "Android 開發環境五步驟建置教學：安裝 JDK 與 Eclipse、下載 Android SDK、設定 ADT 外掛、選擇 SDK 版本套件，最後建立模擬器。"
date: 2012-04-18
categories: ['Android']
---


最近電腦重裝的關係，順便把 Android 開發環境整理一下，

不得不說 Android 在 Eclipse 上的外掛開發的越來越不錯了！

Step1. 到 [Oracle Java SE downloads][1] 點擊 Java Platform (JDK) 並選擇作業系統

Step2. 安裝 [Eclipse IDE for Java EE Developers][2]

Step3. 到 [Android Developers][3] 下載 [Android SDK][4] 並安裝  [![][5]][6]

Step4. 安裝 ADT Plugin for Eclipse ( [官方文件][7] )

   a. 開啟 Eclipse， [Help] -> [Install New Software....]
   
   b. 點選 [Add] 輸入名稱及網址 ( APT / [https://dl-ssl.google.com/android/eclipse/][8] )
   
   c. 等待 pending 結束後會看到可以勾選的 Developer Tools ，選擇 next 並 accept 開始安裝 
   
[![][9]][10]

Step5. 安裝 packages (Platforms and Other Packages)

   a. 從 Eclipse : [Window] -> [Android SDK Manager]
   
   b. 從 Windows : 到 Android SDK 資料夾中點擊 SDK Manager.exe 
   
   c. 從 Linux/Mac : 到 Android SDK 的 tools 資料夾，並執行 android 指令
   
   選擇要開發的版本或 package 並安裝 
   
[![][11]][12]

	沒記錯的話安裝完 Eclpise 的外掛後，Eclipse 會自動跳出問你的 Android SDK 裝在哪，

  若這時候已經安裝好 SDK 的話就可以選擇第二個選項，並指定資料夾位址  
  
[![][13]][14]

接著就可以寫程式了

開啟模擬器的話就到下圖所示新增即可

[![][15]][16]



[1]: http://oracle%20java%20se%20downloads/
[2]: http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/indigosr2
[3]: http://developer.android.com/
[4]: http://developer.android.com/sdk/index.html
[5]: http://4.bp.blogspot.com/-B4zRQJ_Hino/T47FNembF_I/AAAAAAAAC5w/onWCdvf2iC8/s640/android1.png
[6]: http://4.bp.blogspot.com/-B4zRQJ_Hino/T47FNembF_I/AAAAAAAAC5w/onWCdvf2iC8/s1600/android1.png
[7]: http://developer.android.com/sdk/eclipse-adt.html#installing
[8]: https://dl-ssl.google.com/android/eclipse/
[9]: http://3.bp.blogspot.com/-QHyb2NYl9Bc/T47FOa_Pj0I/AAAAAAAAC50/Yrskz6l_VrU/s640/android2.png
[10]: http://3.bp.blogspot.com/-QHyb2NYl9Bc/T47FOa_Pj0I/AAAAAAAAC50/Yrskz6l_VrU/s1600/android2.png
[11]: http://2.bp.blogspot.com/-K8t_6L2xiBs/T47FOxvM8BI/AAAAAAAAC6A/a7WIq7z-ygw/s640/android3.png
[12]: http://2.bp.blogspot.com/-K8t_6L2xiBs/T47FOxvM8BI/AAAAAAAAC6A/a7WIq7z-ygw/s1600/android3.png
[13]: http://2.bp.blogspot.com/-6bJrhcn2SiM/T47GecGxajI/AAAAAAAAC6Q/eneUabD0cmE/s640/android4.png
[14]: http://2.bp.blogspot.com/-6bJrhcn2SiM/T47GecGxajI/AAAAAAAAC6Q/eneUabD0cmE/s1600/android4.png
[15]: http://2.bp.blogspot.com/-aB1Eh5AFUqs/T47HFnNpO9I/AAAAAAAAC6Y/4QhPHIZZcTk/s640/android5.png
[16]: http://2.bp.blogspot.com/-aB1Eh5AFUqs/T47HFnNpO9I/AAAAAAAAC6Y/4QhPHIZZcTk/s1600/android5.png
