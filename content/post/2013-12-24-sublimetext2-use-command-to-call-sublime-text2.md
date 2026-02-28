---
title: '[SublimeText2] use command to call sublime text2'
description: "建立 symlink 讓 Sublime Text 2 可從終端機用 sublime 指令直接開啟檔案或整個目錄，提升工作流效率。"
date: 2013-12-24
slug: sublimetext2-use-command-to-call-sublime-text2
tags: ['tool']
---
```
ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime
```

in terminal:
```
$ sublime filename
$ sublime . (open the directory)
```
