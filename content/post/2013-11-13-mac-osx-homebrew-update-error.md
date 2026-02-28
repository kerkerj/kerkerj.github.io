---
title: '[Mac OSX] homebrew update error'
description: "Homebrew update 因本地 git 變更衝突而失敗的解法：進入 /usr/local 執行 git fetch origin 再 git reset --hard origin/master 即可修復。"
date: 2013-11-13
slug: mac-osx-homebrew-update-error
tags: ['git', 'macos']
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
