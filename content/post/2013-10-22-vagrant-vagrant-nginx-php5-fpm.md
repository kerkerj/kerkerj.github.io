---
title: '[Vagrant] 使用 Vagrant 安裝 Nginx, php5-fpm, MySQL'
description: "使用 Vagrant 搭配 VirtualBox 架設 Nginx+php5-fpm+MySQL 開發環境的完整教學，涵蓋基本指令、Vagrantfile 埠轉發與資料夾同步設定。"
date: 2013-10-22
categories: ['Linux', 'Nginx', 'DevOps', 'PHP']
---


## [Vagrant][1] - Development environments made easy.

就是這個軟體的宗旨，把它想做是 ghost 吧！我們開發網站或是測試一些多機器架構時，常不小心就把自己的電腦或是測試主機搞到爛掉，租機器又慢又花錢，搞到爛重灌更麻煩。因此這個軟體基於 VirtualBox 做出了令人方便設定的功能，讓我們可以快速架設安裝環境並測試，尤其是以多機器架構而言更是方便！例如從最簡單的 Web server + DB server，或是 Web Load Balancer + Application Server * 5 等等的架構，一台電腦就能達成囉！
今天主要是以安裝 Vagrant 及架設出 Nginx + php5-fpm 的架構，下一篇打算另外建構一台 mongodb server，達到 Vagrant 最主要的多機器設定功能~  
  
## Vagrant 基本設定

1. 先下載 VirtualBox 吧！  
2. 下載 Vagarnt 套件 [http://downloads.vagrantup.com/][2]  
安裝好後，可能會需要加 path，至少 1.3.0 當時我是自己加的，現在 1.3.5 就不清楚了~  
3. 接下來就可以開始加入 box (可以把它想為 ghost 檔)，並開始設定 Vagrant。

```
vagrant box add {你想要的Box名稱} {下載網址} 
```

輸入後就會開始下載該 box 了！ 範例：

```
$vagrant box add devbox http://ﬁles.vagrantup.com/precise64.box   
$mkdir my_box  
$cd my_box  
$vagrant init devbox (將 my_box 這個專案資料夾以 devbox 這個 box 檔案初始化)  
```

box 官方有提供 Ubuntu 的，在 [vagrantbox.es][3] 很多，可以自己找～ 當然自己做應該也是可以的！  

## Vagrant 基本指令

```
vagrant up 開機  
vagrant ssh 登⼊  
vagrant suspend 暫停  
vagrant halt 關機  
vagrant destroy 刪除  
```

## 進入新裝好的機器吧！

```
$vagrant up  //開機  
$vagrant ssh //登入  
```

## Vagrantfile 設定
Vagrantfile 這個設定檔可以用很多很強大的設定，在官方網頁裡有教學

不過首先要先做的是： `config.vm.forward_port 80, 8088 `

把虛擬機器裡從 80 port 傳送的東西丟到本機的 8088 port

如此一來在本機瀏覽器輸入 localhost:8088 ，就可以看到網頁了！

另外就是： `config.vm.synced_folder "/vagrant", "本機目錄" `

如此一來，Ubuntu 底下就有一個 /vargrant 目錄，和自己主機裡的某個目錄是同步的！

也就是在自己主機上寫好程式後，就可以直接讓虛擬機器抓到 code 去執行，超方便的！

只要把 nginx 的網頁目錄設定在 /vagrant 上就可以了！ (後面會說)  
  
## Ubuntu 基本安裝

進去後是乾淨的 Ubuntu 12.04 64位元版，因此需先做點安裝:

```
$sudo apt-get -y update  
$sudo apt-get --no-install-recommends -y install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev libgdbm-dev ncurses-dev automake libtool bison subversion pkg-config libffi-dev vim  
```

有些東西其實可以不用裝，就看個人的需求，必裝應該是 build-essential, openssl, curl, git-core, vim XD

## Nginx & php5-fpm 安裝

安裝 Nginx

```
$sudo apt-get install nginx  
```

若要更改 Nginx 的網頁目錄，設定檔在 `/etc/nginx/` 裡，

通常應該是更改 `/etc/nginx/sites-enable/default` 裡的 `root /usr/share/nginx/www/`

還要將這段 code 解除註解：

```
location ~ \.php$ {
	fastcgi_split_path_info ^(.+\.php)(/.+)$;
	# NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini

	# With php5-cgi alone:
	#fastcgi_pass 127.0.0.1:9000;
	# With php5-fpm:
	fastcgi_pass unix:/var/run/php5-fpm.sock;
	fastcgi_index index.php;
	include fastcgi_params;
}
```

更改後確認一下 `/etc/nginx/nginx.conf` 是否有 `inculde /etc/nginx/sites-enable/*`

```
$sudo service nginx start (開始服務)
```

安裝 php5-fpm

```
$sudo apt-get install php5-fpm  
$sudo vim /etc/php5/fpm/php.ini (找到並更改 cgi.fix_pathinfo = 0 )  
$sudo vim /etc/php5/fpm/pool.d/www.conf  
```

找到 `listen = 127.0.0.1:9000`, 換成 `/var/run/php5-fpm.sock`
寫個 phpinfo(); 的 php 檔放入資料夾

```
$sudo service php5-fpm restart (開啟服務)
```

在瀏覽器輸入 localhost:8088 就可以看到 nginx 的頁面，再讀取剛剛寫的 php 應該就成功囉！ 

安裝 MySQL:

```
$ sudo apt-get install mysql-server
```

過程中會問 root 的密碼，裝完後就可以登入：

```
$ mysql -u root -p
```

下回就是多機器的設定囉～


reference:  
[http://gogojimmy.net/2013/05/26/vagrant-tutorial/][4] 

[http://www.slideshare.net/ihower/vagrant-osdc][5] 

[https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04][6]  
  
p.s. Vagrantfile 產生器：[Puphpet.com/#vagrantfile][7] 

這網站有很多值得玩的地方，可以直接幫你產生出這些設定檔真的是太方便了！  
  
p.s.s.有亂入用到的指令記一下：dpkg --get-selections 列出已安裝的 package 

另外提供一下我在研究 Vagrant 的筆記，很亂超級亂，但是有些東西沒有寫在 blog 裡~
[Evernote by kerkerj][8]  


[1]: http://www.vagrantup.com/
[2]: http://downloads.vagrantup.com/
[3]: http://www.vagrantbox.es/
[4]: http://gogojimmy.net/2013/05/26/vagrant-tutorial/
[5]: http://www.slideshare.net/ihower/vagrant-osdc
[6]: https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04
[7]: https://puphpet.com/#vagrantfile
[8]: https://www.evernote.com/shard/s81/sh/91ccf490-d9cd-468e-943a-d57c79d052f4/71e0a682b024c6e97fd8c79b90f89f67
