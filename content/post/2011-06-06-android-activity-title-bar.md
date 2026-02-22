---
title: '[Android] 移除 Activity的 Title bar'
description: "本文示範如何在 Android 應用程式中移除 Activity 的標題列，透過在 `AndroidManifest.xml` 中設定 `android:theme` 屬性即可達成。"
date: 2011-06-06
categories: ['Android']
---


```xml
<activity android:name=".MyMainClass"
	          android:label="@string/app_name"
	          android:theme="@android:style/Theme.NoTitleBar">
</activity>
```

style/Theme.NoTitleBar
