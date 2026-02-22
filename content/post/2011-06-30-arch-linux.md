---
title: '[Arch linux] 安裝雜記'
description: "本文紀錄 Arch Linux 安裝過程，涵蓋硬碟準備、軟體包選擇、系統配置 (`rc.conf` 與 `locale.gen` 編輯)、引導載入器安裝、使用者設定及 `sudo`、`screen`、`ssh` 和 KDE 桌面環境的建立。"
date: 2011-06-30
categories: ['arch linux']
---


Arch linux 安裝雜記  
本來要研究如何弄到USB上安裝的, 但發現教室有光碟, 所以就沒去研究了  
以下是安裝Arch linux的一點簡單紀錄  

首先不得不說[Arch linux的維基][1]  
用關鍵字查詢會有許多結果, 包含中文翻譯  
雖然目前還是簡體字較多, 但總比直接看英文還來的快些  
若安裝上有問題或是對某個套件的詳細內容有興趣可以到這查  

我這次安裝主要是先參考此篇文章: [ArchLinux 推廣教學起跑！(09.7.5更新)][2]  
寫得很詳細, 雖然是2009年的文章, 但仍非常有參考價值  

前半段的步驟會和此篇文章有點類似  

1. 使用光碟開機後  
進入 arch linux 安裝介面, 輸入 arch 或是 root 登入,  
再輸入 `/arch/setup` 進入安裝程式  

2. 印象中流程和此篇文章不太一樣, 但是就是照螢幕上所提示的, 你的狀況是什麼就選擇什麼  
例如若是用光碟安裝就選擇用光碟安裝, 一步一步由上而下照著主選單目錄作：  

	- Prepare Hard Drive: 我是使用整顆硬碟所以不需要太繁複的步驟  
	- Select Package: 我使用預設值, 也不需要什麼步驟  
	- Install package: 不能跳著作,一定要選擇package後才能安裝,沒選擇package也不會繼續往下作  
	- Configure System: 照網頁作, 他問什麼就回答自己的情況就好  
	- 文字編輯器我選擇 vi (其實之後裝了vim,我比較熟悉vim,不會用vi的話要選nano)  
	- 必須先編輯兩個檔案: `/etc/rc.conf`   `/etc/locale.gen`  
	- `rc.conf` 是開機時會讀取的檔案  
	- 因此要開機啟動的服務, 網路設定等等設定都會寫在這裡面, 很重要的檔案  
	- 若是固定 IP 的話照著後面的設定 ( rc.conf 裡應該會有 example, 先找看看有沒有相關的字再編輯)  
  - 接下來編輯 `/etc/locale.gen`  
  - 把中文相關的 `zh_TW `取消註解(去掉最前面的#)  
  - 還要設定 root 密碼  

Install Bootloader  
	- 就照預設安裝吧, 會要你選擇要裝在哪, 我是裝在第一個0.0  
Exit Install  
	- 完成後就離開囉, 然後輸入 reboot 重開機  

3. 進入新系統, 建立新使用者 `#adduser`  
  
4. 安裝sudo, screen, ssh  
`#pacman -S sudo openssh screen`  
ssh 的設定可以參考 Arch linux 的 wiki:[SSH (簡體中文)][3]  
  
5. 若要安裝桌面環境的話就自行google吧  
個人有安裝最小安裝的KDE  
`#pacman -S kdebase-workspace kdebase-konsole`  
另外加裝了firefox, 主要是因為專題同學不習慣使用命令列  
因此還是裝了個環境讓有急用時可以上網搜尋之類的  
Arch linux wiki 上關於 KDE 的頁面是正體中文  
因此可以參考看看 [KDE (正體中文)][4]  

下篇將會紀錄在 arch linux 上運作 Git 的服務  


## 固定 IP 設定

```
	(IP亂打的XD)
	#Static IP example
  eth0="eth0 266.100.200.3 netmask 255.255.255.0 broadcast 266.100.200.255"
  #eth0="dhcp"
  INTERFACES=(eth0)
  
  # Routes to start at boot-up (in this order)
  # Declare each route then list in ROUTES
  # - prefix an entry in ROUTES with a ! to disable it
  #  
  gateway="default gw 266.100.200.254"
  ROUTES=(gateway)
```


[1]: https://wiki.archlinux.org/index.php/
[2]: http://antimalicious.blogspot.com/2009/04/archlinux.html
[3]: https://wiki.archlinux.org/index.php/SSH_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
[4]: https://wiki.archlinux.org/index.php/KDE_(%E6%AD%A3%E9%AB%94%E4%B8%AD%E6%96%87)
