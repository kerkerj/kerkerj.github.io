---
title: '[Android] thread 處理 UI update (2)'
description: "Android 背景執行緒更新 UI 的進階做法：使用 AsyncTask 替代 runOnUiThread，程式結構更清晰，附 GitHub gist 程式碼範例。"
date: 2012-06-12
slug: android-ui-update-2
tags: ['android']
---


這是第一篇: [[Android] thread 處理 UI update]

在那之後自己在寫一些東西時也用到 runOnUiThread  

不過有別的方法可以不用像 runOnUiThread 寫的比較雜亂  

那就是用 AsyncTask , 剛剛自己也順手寫了一個 Test 放在 Github 上  

我覺得使用 AsyncTask 可以不用處理 thread, 而且寫法上比較清楚~  

<script src="https://gist.github.com/kerkerj/7367244.js"></script>
