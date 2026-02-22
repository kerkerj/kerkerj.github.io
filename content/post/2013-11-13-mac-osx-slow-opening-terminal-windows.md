---
title: '[Mac OSX] Slow-Opening Terminal Windows'
description: "本文探討 Mac OSX 終端機開啟緩慢的問題，解釋 `Terminal.app` 預設載入 `.bash_profile` 的特性，並提供在 `.bash_profile` 中直接定義 `PATH` 變數以顯著提升終端機啟動速度的解決方案。"
date: 2013-11-13
categories: ['macOS', 'Linux']
---


最近在開 mac 的 iTerm.app 或者是內建的終端機都覺得卡卡的，

之前以為是 .bashrc 載入太多東西導致的，所以把一些掛載的 bin 目錄都註解掉，

但是還是沒解決，心想不對勁，就順手 google 了一下...

我執行了下列語法：

```
$ time /usr/libexec/path_helper
PATH="/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Users/ABC/android-sdks/platform-tools:/Users/ABC/android-sdks/tools:/Application/Vagrant/bin"; export PATH;

real	0m0.043s
user	0m0.001s
sys	0m0.002s
```

接著我把 `PATH` 的內容寫進 `.bash_profile`，問題竟然就解決了...

速度飛快～～

後來看到一篇 [.bash_profile vs.bashrc](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html)

雖然大家都知道 `.bash_profile` 是在登入時執行（即是輸入帳號密碼時），

而 `.bashrc` 是在系統內非登入狀態時開啟 prompt 時執行，

但是！！ Mac OSX 是個例外～是個例外～是個例外～.......=.=

>Mac OS X — an exception

>An exception to the terminal window guidelines is Mac OS X's Terminal.app, which runs a login shell by default for each new terminal window, calling .bash_profile instead of .bashrc. Other GUI terminal emulators may do the same, but most tend not to.

所以通常解決方法是在 `.bash_profile` 寫入以下 script:

```
if [ -f ~/.bashrc ]; then
	source ~/.bashrc
fi
```

不過我之前就這樣做了，這次成功的方式是直接在 `.bash_profile` 寫入 PATH 路徑...

猜想應該是在 `.bash_profile` 預先讀取而加快速度的

如果把原本的 `.bashrc` 的 `PATH` 註解掉都移到 `.bash_profile` 會再稍快一些
