---
title: "Add tracked file to .gitignore"
description: "已被 Git 追蹤的檔案如何加入 .gitignore：本機用 git update-index --assume-unchanged，若有遠端同步還需執行 git rm --cached。"
date: 2014-08-15
slug: add-tracked-file-to-gitignore
tags: ['git']
---


```
git update-index --assume-unchanged <file>
```

If there's a remote server, also do:

```
git rm --cached <file>
```
