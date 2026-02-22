---
categories: ['Hugo', '筆記']
date: 2015-05-03T02:14:59+08:00
description: "Migrating to Hugo from Octopress"
tags: ["hugo", "octopress", "notes"]
title: "Migrating to Hugo from Octopress"
---


<!--toc-->

# [hugo](https://gohugo.io)

<l><b>hugo - 快速又現代的靜態網站產生器</b></l>

^^^ 這是 spf13 自己說的

繼 spf13-vim 後又一個 spf13 出品的好東西 XD

在靜態網站產生器中最廣為人知的應該就是 [`Jekyll`](http://jekyllrb.com/)

以及基於 `Jekyll` 的 [`Octopress`](http://octopress.org/) 了

(關於靜態網站產生器，[這篇文章](http://www.sitepoint.com/6-static-blog-generators-arent-jekyll/) 介紹了六個除了 `Jekyll` 以外的產生器)

## 為什麼要用 hugo？

雖然 `Jekyll` / `Octopress` 很紅資源多主題也不少

但是缺點就是要使用它們就必須裝 `ruby`、裝 `gem`

有在寫 `ruby` 的人感覺應該還好

沒在寫 `ruby` 的光想像就覺得應該會被搞死... XD

而且覺得文章一多時在編譯的時候好慢...

用 `hugo` 的好處就是因為他是用 `go` 寫的

執行速度飛快，而且只要下載一個 `binary` 檔案後就可以操作了喲~ 

## 基本 hugo 安裝與操作

首先先安裝 `hugo` (我是用 `mac` 的 `homebrew`)

```
$ brew install hugo
```

使用 `hugo` 產生一個新的網站

```
$ hugo new site /path/to/the/site
e.g.
$ hugo new site /Users/kerkerj/my_hugo_blog
```

到剛剛產生的網站目錄底下

```
$ cd /path/to/the/site
e.g.
$ cd /Users/kerkerj/my_hugo_blog
```

產生一些內容

```
$ hugo new about.md
```

或是一篇文章

```
$ hugo new post/first-post.md
```

安裝主題

```
$ git clone --recursive https://github.com/spf13/hugoThemes themes
```

把 `hugo` 跑起來!

```
$ hugo server --theme=hyde-x --buildDrafts --watch
```

指令說明:

`--theme=hyde-x` 

>指的是使用哪一個主題，可以在 `themes` 資料夾裡找，並將 `hyde` 替換掉就可以換主題，例如換成 `greyshade`

`--buildDrafts` 

>使用 `hugo new post/first-post.md` 預設會是草稿 (在 `first-post.md` 裡會有 `draft=true`)

>如果下 `--buildDrafts` 的意思就是會把草稿也 build 出來讓你看到

`--watch`

>如果在 server 開啟時，編輯文章並儲存的話，server 會幫你根據修改的內容重新產生網頁，

>不需要再下指令重新 build 一次

## Migrating from octopress

我自己原本是用 `octopress`

為了不要怕改壞原本的 `octopress` 文章

我把在 `octopress` 的文章 (`source/_posts`) 

全數複製一份到 `my_hugo_blog/content/post/` 底下

接著就試著跑 `hugo server` 起來試試看

當然是炸了一堆 ERROR 出來啦 XD

以下是我遇到的幾個常見問題

## Meta-data 的 Date 

這是我的錯誤訊息

```
ERROR: 2015/05/02 Failed to parse date '2010-10-04 18:25' in page post/2010-10-04-freebsd81-famp.md
...
```

首先瞭解一下文章的 meta-data 格式

`octopress` 的是 `yaml` 格式

```
---
title: "Setup wifi on raspberry pi2"
date: 2015-04-22 22:59:11 +0800
comments: true
categories: [raspberry]
keywords: [raspberry, pi, pi2, wifi]
description: 
---
```

`hugo` 的則是 `toml` 格式

```
+++
date = "2015-05-02T02:27:21+08:00"
draft = true
title = "test"

+++
```

現在 `hugo` 0.13 版開始也可以讀 `yaml` 格式了

(在 config 中可以設定 `metaDataFormat: "yaml"`)

不過比較麻煩的是 `date` 這個屬性，兩邊長得並不太一樣

`octopress` 比較舊的是長這樣 `2010-10-04 18:25`

新一點 (某一次更新後) 的長這樣 `2014-10-02 01:37:44 +0800`

不過不管怎樣，`hugo` 都不吃

因為 `hugo` 的長這樣 `2015-05-02T02:27:21+08:00`

囧了

因此必須將 `octopress` 的格式換成 `hugo` 的

接下來下的指令最好先 `git commit` 一下後再做 XD

```
$ cd my_hugo_blog/content/post/ 
$ find . -type f -exec sed -i "" -e 's/date: \([0-9]\{4\}-[0-9]\{2\}-[0-9]\{2\}\).*$/date: \1/g' {} \;
```

這個指令會將原本的 date 格式換成只有 `yyyy-mm-dd` 的格式

例如 `2014-10-02 01:37:44 +0800` 換成只有 `2014-10-02` 

這樣 `hugo` 就看得懂

## permallink 的問題

當日期問題修好後，重開 server

發現點擊文章標題進單篇文章網頁時會跳 404

原因是 `hugo` 預設產生的連結是 

`http://yoursite.com/post/2015-04-22-raspberry-pi2-remote-webcam/`

而在 `octopress` 是根據日期區分

`http://yoursite.com/post/2015/04/22/raspberry-pi2-remote-webcam/`

這個問題的解法比較簡單

在 `config.toml` 加入這行就行

```
[Permalinks]
	post = "/:year/:month/:day/:filename/"
```

不過因為 `octopress` 本身在產生檔案時就已經把日期加入檔名中 

(e.g. `2015-04-22-raspberry-pi2-remote-webcam.md`)

因此 `octopress` 的文章連結看起來就會很長，例如:

```
http://localhost:1313/2015/04/22/2015-04-22-raspberry-pi2-remote-webcam/
```

這時候就看要不要把檔名改掉啦

例如把 `2015-04-22-raspberry-pi2-remote-webcam.md`

改成 `raspberry-pi2-remote-webcam.md`

或者可以試著改成:

```
[Permalinks]
	post = "/:year/:month/:day/:title/"
```

不過我試了一下，會有機會因為標題有特殊符號或中文

```
e.g.  http://localhost:1313/2014/10/20/swift-d20---basic---%E6%8D%A8%E6%A3%84-storyboard-%E4%BD%BF%E7%94%A8%E7%B4%94%E7%A8%8B%E5%BC%8F%E7%A2%BC-%E7%9A%84%E6%96%B9%E5%BC%8F%E6%92%B0%E5%AF%AB-viewcontroller/
```

產生出奇怪的資料夾名稱而導致該文章無法存取 

所以這個方式僅供參考，可以試試 XD

BTW，我個人在撰寫文章時都沒有用到產生器的樣板語言，因此在轉換時比較沒遇到什麼問題

上述問題解法的參考來源:

[Migrating to Hugo From Octopress](http://nathanleclaire.com/blog/2014/12/22/migrating-to-hugo-from-octopress/)

[OctopressからHugoへ移行した](http://deeeet.com/writing/2014/12/25/hugo/)

## 設定檔 (config.toml, config.yml, config.json)

`hugo` 預設的是 `config.toml`，也支援 `yaml` 和 `json`

而 `hugo` 讀取的順位是照上述順序

如果是要將網頁讓 `GitHub` host 的話，就必須使用 `config.yaml`

因此我就把 `config.toml` 換成 `config.yml` 了 (也比較習慣)

以下是我的 `config.yml` 主要的設定

```
baseurl:            "http://blog.kerkerj.in/"
languageCode:       "en-us"
title:              "My New Hugo Site"

buildDrafts:        false
config:             "config.yml"
metaDataFormat:     "yaml"

permalinks:
  post:             "/blog/:year/:month/:day/:filename/"

theme:              "purehugo"
```

設定 `metaDataFormat: "yaml"` 的話

使用 `hugo new` 的文章 meta-data 都會是 `yaml` 格式喔

要查詢預設的參數在這 [Configuring Hugo](http://gohugo.io/overview/configuration/)

## 主題佈景 Theme

其實在 `hugo` 裡比較讓我覺得麻煩的還是主題的挑選吧

因為選擇實在不多

我最喜歡的是在 `octopress` 中的 [greyshade](https://github.com/shashankmehta/greyshade)

雖然 `spf13/hugoThemes` 的 repo 中有人 porting，但是完成度似乎不高

改 template 是應該沒什麼難度，照著改就好，不過...實在沒這麼多時間 Orz

而且 `hugo` 在二月底 release 了 0.13 版，有一部分的主題佈景都不能動了 XD

這邊列出我試過可以 work 的

* [herring-cove](https://github.com/spf13/herring-cove)
* [hugo-uno](https://github.com/SenjinDarashiva/hugo-uno)
* [hyde-x](https://github.com/zyro/hyde-x)
* [purehugo](https://github.com/dplesca/purehugo)
* [redlounge](https://github.com/tmaiaroto/hugo-redlounge)
* [simple-a](https://github.com/AlexFinn/simple-a)
* [tachyons](https://github.com/marloncabrera/tachyons)
* [tinyce](https://github.com/roperzh/tinyce-hugo-theme)
* [vienna](https://github.com/keichi/vienna) 

`vienna` 的 index list 壞掉了，不過 single page 能動，佈景也還算漂亮

自己修一下應該就可以

其實其他不少佈景也都是 index page 不能動，其他都能動

另外在試的過程中，發現除非 repo 有特別說

不然 `config.yml` 的 `disqus` 的 key 都是 `disqusShortname`

(因為有個佈景的 key 是 `disqus_short_name`)

`hyde-x` 是最多人 star 的主題佈景，有八種色系可以選擇

功能也蠻齊全，不想自己搞東搞西的話，建議就直接選這個主題

(因為我最後 [fork 人家的](https://github.com/dplesca/purehugo) theme [自己改了一堆](https://github.com/kerkerj/purehugo) (遮臉) )

## Syntax highlight

寫技術筆記的人應該對這個非常執著 XD

我使用的主題是 `purehugo`

不過他的 syntax highlight 是預設的常見的模式

因此我就自己把他抽掉換成 [`highlight.js`](https://github.com/isagalaev/highlight.js)

`hugo` 在 [這篇文件](http://gohugo.io/extras/highlighting/) 中有另外說明關於如何自訂 syntax highlight

## 支援 (disqus, ga, pagination, SEO, robot.txt, favcon)

`disqus` 要看佈景有無支援，`GA` 也是

不過如果主題沒有的話，也是自己寫一個 partial 插進去就好

另外 `pagination` 也是在最新版 (0.13) 才加入，所以也要看佈景有沒有支援

不然文章多的就會發現首頁往下捲捲不完... XD

`SEO` 的部分有人特別寫 partial [nozzle/hugo-snippets](https://github.com/nozzle/hugo-snippets)

自己寫在 theme 的 header.html 當然是最直覺的

`robot.txt`、`favcon` 就自己加進 `static` 資料夾，再到 theme 裡面修改

## Deploy

`hugo` 有一篇 [教學文](http://gohugo.io/tutorials/github-pages-blog/) 教你如何放在 `GitHub` 上

有兩種方式 (Project page or Personl page)

不過都需要先在 `config.yml` 加入以下資訊

```
---
contentdir: "content"
layoutdir: "layouts"
publishdir: "public"
indexes:
  category: "categories"
baseurl: "http://kerkerj.github.io/ or http://kerkerj.github.io/hugo_gh_blog/"
title: "Hugo Blog Template for GitHub Pages"
canonifyurls: true
```

### Project page: e.g. http://your_github_account.github.io/hugo_gh_blog/

先處理好本機端 `hugo` 的 blog，接著 `git init` 並且 `commit` 後

加入 `gh-pages` 這個 branch ([教學](https://help.github.com/articles/creating-project-pages-manually/))

接著回到 `master`，照著下列步驟

```
# Fetch the deployment script into the root of your source tree, make it executable.
wget https://github.com/X1011/git-directory-deploy/raw/master/deploy.sh && chmod +x deploy.sh

# For setting it up to build to a folder other than "dist", see the options in deploy.sh.
# Build the site to /dist.
hugo -d dist

# Run the deploy.sh script installed above.
./deploy.sh
```

就可以 deploy 到 github project page 了

存取網址就是 `http://your_github_account.github.io/hugo_gh_blog/`

### Personal page: e.g. http://kerkerj.github.io/

建立兩個 repo，一個拿來放 `my_hugo_blog` 本機端的資料

一個就是 `your_github_account.github.io` 這個 repo 

會拿來放 `my_hugo_blog/public` 資料夾裡的資料 

所以就必須先在 `GitHub` 上有 `your_github_account.github.io` 這個 repo

這邊會比較簡單

以剛剛的例子的話就會是 `my_hugo_blog`，這個資料夾裡放了所有剛剛 `hugo` 產生的資料

先處理 `deploy.sh`:

可以直接自己把以下內容存成 `deploy.sh`，並 `chmod +x deploy.sh` 使其可執行

```
#!/bin/bash
# Deploy hugo site to GitHug personal page
 
echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"
 
# Build the project. 
hugo # if using a theme, replace by `hugo -t <yourtheme>`
 
# Go To Public folder
cd public
# Add changes to git.
git add -A
 
# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"
 
# Push source and build repos.
git push origin master
 
# Come Back
cd ..
```

或下載 `deploy.sh` (同時使其可執行)

```
wget https://gist.githubusercontent.com/kerkerj/18b18a24af8e8a0ec0ee/raw/654dfb7faaa23db24f707c4d746319e458157d89/deploy.sh && chmod +x deploy.sh
```

如果有使用 theme，記得修改 `deploy.sh`

把 

```
hugo # if using a theme, replace by `hugo -t <yourtheme>`
```

換成

```
hugo -t yourtheme # if using a theme, replace by `hugo -t <yourtheme>`
```

所以現在已經處理好的東西有:

1. `my_hugo_blog ` 資料夾以及可執行的 `deploy.sh`
2. 已經在 `GitHub` 上有 `your_github_account.github.io` 這個 repo

(p.s. 如果有要設定 CNAME 什麼的，就把 CNAME 檔案放 `static` 這個資料夾裡)

接下來，先將 server 跑起來

```
$ hugo server --watch -t <yourtheme>
```

把你想要的網站調整好後，關掉 server 並將 `public` 資料夾刪除 (等等會重新在 deploy.sh 中產生)

```
$ rm -rf public
```

接著把 `GitHub` 的 repo 加到 `public` 裡

```
$ git submodule add git@github.com:<username>/<username>.github.io.git public
```

最後就 deploy 到 github 上

```
# 自動產生 commit message
$ ./deploy.sh 

# 如果要自訂 commit message 的話
$ ./deploy.sh "Your commit message"
```

如果是第一次 deploy 到 `GitHub` 的話，需要等待大約 15 ~ 20 分鐘

才會在 `GitHub` 上看到你的網站喲

