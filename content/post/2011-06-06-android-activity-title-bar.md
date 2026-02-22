---
title: '[Android] 移除 Activity 的 Title bar'
description: "Android 移除 Activity 標題列的快速設定，在 AndroidManifest.xml 的 activity 標籤加上 Theme.NoTitleBar 即可。"
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
