---
title: "Setup wifi on raspberry pi2"
description: "Raspberry Pi 2 WiFi 設定教學（含 2021 更新），依 kernel 版本下載對應驅動程式，並設定 wpa_supplicant.conf 連接 WiFi 網路。"
date: 2015-04-22
categories: ['Raspberry Pi']
---

<font color="red">2021-0527 更新</font>

1. 先確認自己的 kernal 版本

   ```
   $ uname -r
   5.10.17-v7+
   ```

2. 到 http://downloads.fars-robotics.net/ 裡找相對應版本

   ```
   http://downloads.fars-robotics.net/
   	wifi-drivers/
   		8188eu-drivers/ 
   			...
   			8188eu-3.18.7-757.tar.gz
   			...
   			8188eu-4.1.15-829.tar.gz
   			....
   			8188eu-5.10.17-v7-1414.tar.gz
   ```

   檔案命名的規則是 8188eu-{$kernal_version}-xxxx.tar.gz

3. 找到相對應的 kernal 版本後下載下來並解壓

   ```
   $ wget http://downloads.fars-robotics.net/wifi-drivers/8188eu-drivers/8188eu-5.10.17-v7-1414.tar.gz
   $ tar xvf 8188eu-5.10.17-v7-1414.tar.gz
   ```

4. 執行 install.sh 後重開機應該就有了

   ```
   $ ./install.sh
   ```

若沒有的話，翻翻 http://downloads.fars-robotics.net/ 的資料看有沒有提供解方

<hr>

(這是篇筆記)

前陣子從前同事那接手了一塊 raspberry pi2 的板子

想要用 wifi 來連網，於是就買了一個 usb 無線網卡

型號是:

TP-LINK TL-WN725N 150MbpsUSB無線網卡 ([pchome連結](http://24h.pchome.com.tw/prod/DRAF4Z-A75333252))

一開始當然還是必須先插網路線，`ssh` 進去後

先檢查 `pi2` 的 `kernel` 版本

```
$ uname -r
3.18.7-v7+ # 這是我的版本
```

接著根據這個 [repo](https://github.com/lwfinger/rtl8188eu/) 

將韌體載下來放到 /lib/firmware 裡

```
$ sudo wget https://github.com/lwfinger/rtl8188eu/raw/c83976d1dfb4793893158461430261562b3a5bf0/rtl8188eufw.bin -O /lib/firmware/rtlwifi/rtl8188eufw.bin 
```

再來設定 pi2 的網路

```
$ ifconfig
eth0      Link encap:Ethernet  
lo        Link encap:Local Loopback
wlan0     Link encap:Ethernet 
```

如果網卡有裝成功，應該就會有 `wlan0` 或是 `wlanX` 之類的 (X 是數字)

最後就是手動掃描無線網路，並設定連線值囉

```
$ sudo iwlist scan => 掃描無線網路 AP
wlan0     Scan completed :
          Cell 01 - Address: XX:XX:XX:XX:XX:XX
                    ESSID:"GGININDER"
          Cell 02 - Address: XX:XX:XX:XX:XX:XX
                    ESSID:"TP-LINK_F5566"
```

連線的 `SSID` 就找 `Cell` 裡的 `ESSID` 

(通常應該會記得自己家的 AP 啦，可以用來確認一下)

開啟 `/etc/network/interfaces` 來寫入連線值

我的 AP 的加密機制是 WPA/WPA2，所以使用下面的方式連線

若是其他方式 (e.g. WEP) 可以參考 [這篇](http://inpega.blogspot.tw/2013/09/blog-post_15.html)

```
$ vi /etc/network/interfaces
auto lo

iface lo inet loopback
iface eth0 inet dhcp

# 重點是下面這段

# 允許熱插拔 wlan0 這個介面
allow-hotplug wlan0 

# 預設設定為 dhcp
iface default inet dhcp

# 設定 wlan0 這張介面卡為 DHCP 自動取得 IP
iface wlan0 inet dhcp

# 你的 AP 的 SSID
wpa-ssid "GGININDER"

# 你的 AP 的密碼
wpa-psk "password"
```

設定好後，重新啟動 wlan0 介面就可以連線囉

```
$ sudo ifdown wlan0
$ sudo ifup wlan0
$ ifconfig wlan0 # 確認是否取得 IP
```

reference:

raspberry pi forum: http://www.raspberrypi.org/forums/viewtopic.php?f=28&t=62371&start=475

網路設定: http://inpega.blogspot.tw/2013/09/blog-post_15.html

