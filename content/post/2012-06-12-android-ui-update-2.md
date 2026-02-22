---
title: '[Android] thread 處理 UI update (2)'
description: "本文延續 Android 執行緒更新 UI 的討論，介紹 `AsyncTask` 為 `runOnUiThread` 的替代方案，能以更簡潔的方式處理背景操作與 UI 更新，並提供 GitHub 上的程式碼範例。"
date: 2012-06-12
categories: ['Android']
---


這是第一篇: [[Android] thread 處理 UI update]

在那之後自己在寫一些東西時也用到 runOnUiThread  

不過有別的方法可以不用像 runOnUiThread 寫的比較雜亂  

那就是用 AsyncTask , 剛剛自己也順手寫了一個 Test 放在 Github 上  

我覺得使用 AsyncTask 可以不用處理 thread, 而且寫法上比較清楚~  

<script src="https://gist.github.com/kerkerj/7367244.js"></script>
