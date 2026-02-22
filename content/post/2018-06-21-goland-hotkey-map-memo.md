---
title: "Goland Hotkey Map Memo"
date: 2018-06-21
slug: "goland-hotkey-map-memo"
categories: ['Golang', '工具']
tags: ['Goland', 'JetBrains', 'IDE', '快捷鍵']
description: "整理 JetBrains Goland 常用快捷鍵，包含搜尋、導覽、重構等操作，方便日常開發快速查閱。"
draft: true
---

首先從 command line 開啟 （Intellij 是 `idea`）



* cmd + o - 搜尋 project 裡的 Class (e.g. 有個 struct 叫做 `Coordinate`, 輸入 `Coordinate` 即可到該 struct)
* shift + cmd + o - 搜尋檔案名
* alt + cmd + o - 搜尋 Symbol
* cmd + b - 到變數宣導的地方
* cmd + 7 - toggle go structure tree
* cmd + e - 最近開啟的檔案
* cmd + shift + e - 最近變更過的檔案
* alt + F7 - Find usages (沒作用，不知道是啥)
* alt + space - quick definition （衝突了)
* cmd + n - 針對某個 struct 來選擇要實作哪個 interface
* cmd + p - paramter info 在方法參數上按下 cmd + p 會顯示該參數需要的型別
* F1 - 顯示函數的文件
* shift + ctrl + cmd - Expression Type
* alt + enter - show intention action，在有綠色底線的顯示底下輸入則會顯示可以做哪些事
* shift + cmd + a - 顯示 action
* shift + F6 - rename (refactor)
* 選擇一段程式，然後輸入 ctrl + T - extract code
* alt + F12 - toggle terminal view
* cmd + shift + [] - switch tabs (或是使用 ctrl + tab 來開啟 switcher)
* cmd + E - view recent file
* cmd + r - replace mode
* cmd + capslock - reopen closed tab (capslock is change to be Esc)
* 在函式參數上按下 cmd + p - 顯示函式的參數



-------------





* 開關檔案列表 - cmd + 1 
* 開關 go 的結構 - cmd + 7
* 開關 terminal 視窗 - alt + F12
* 顯示函數文件 - F1
* 搜尋檔案名 - shift + cmd + o
* 搜尋字串 - shift + cmd + f (可選擇在 project / module / directory)





Refactor:

1. rename - shift + F6
2. extract - 選取程式區塊後
   1. extract to variable - cmd + alt + v
   2. extract to method - cmd + alt + m
   3. extract to constant - cmd + alt + c
