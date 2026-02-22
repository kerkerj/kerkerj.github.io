---
title: "Add tracked file to .gitignore"
description: "git update-index --assume-unchanged If there's a remote server, also do: git rm --cached"
date: 2014-08-15
categories: ['Git']
---


```
git update-index --assume-unchanged <file>
```

If there's a remote server, also do:

```
git rm --cached <file>
```
