---
title: '[Mac OSX] homebrew update error'
description: "本文針對 Mac OSX 上 `homebrew update` 因本地修改或未追蹤檔案造成的 Git 合併錯誤，提供解決方案：進入 `/usr/local` 執行 `git fetch origin` 後 `git reset --hard origin/master` 以解決衝突。"
date: 2013-11-13
categories: ['Git', 'macOS']
---


failed log:

```
$sudo brew update
error: Your local changes to the following files would be overwritten by merge:
    Library/Aliases/gperftools
    Library/Aliases/hashdeep
    Library/Aliases/htop
    Library/Aliases/nodejs
    Library/Aliases/ocio
    Library/Aliases/oiio
		....
error: The following untracked working tree files would be overwritten by merge:
	Library/Aliases/gperftools
    Library/Aliases/hashdeep
    Library/Aliases/htop
    Library/Aliases/nodejs
    Library/Aliases/ocio
    Library/Aliases/oiio
		....
Aborting
Error: Failure while executing: git pull -q origin refs/heads/master:refs/remotes/origin/master
```

Solution:

```
$ cd /usr/local
$ git fetch origin
$ git reset --hard origin/master
```

[reference](http://stackoverflow.com/questions/10762859/brew-update-the-following-untracked-working-tree-files-would-be-overwritten-by) 
[[read]](http://git-scm.com/2011/07/11/reset.html)
