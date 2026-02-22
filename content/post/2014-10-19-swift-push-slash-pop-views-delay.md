---
title: "Swift - Push/Pop Views delay"
description: "Swift Navigation push/pop 轉場出現殘影的除錯記錄，原因是 UIView 的 backgroundColor 預設為 nil，為目標 view 設定 backgroundColor 後解決。"
date: 2014-10-19
categories: ['Swift', 'iOS']
tags: ['鐵人賽']
---


請大家先看看下面的小短片，注意一下過場動畫的流暢度

一開始是沒有加過場動畫，後來改程式碼變成有過場動畫

<a href="http://www.youtube.com/watch?feature=player_embedded&v=dqxDFv-iieU
" target="_blank"><img src="http://img.youtube.com/vi/dqxDFv-iieU/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="420" height="315" border="10" /></a>

[direct link](https://www.youtube.com/watch?v=dqxDFv-iieU)

後來發現給一個預設的 backgroundColor 後就不會發生殘影的問題了

是因為 push 的 view 沒有 backgroundColor

和同事討論後發現:

[Apple Developer Reference](https://developer.apple.com/library/IOs/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/instp/UIView/backgroundColor)

UIView 的 backgroundColor 的 default 值是 null!

>Discussion
>Changes to this property can be animated. The default value is nil, which results in a transparent background color.

所以是這個原因導致在轉場的時候有殘影 

不過切換 View 時 target view 沒有 backgroundColor 的情況應該比較少吧...(?)

剛好在寫鐵人賽的 app 想先把流程弄出來時遇到了這個雷 XD
