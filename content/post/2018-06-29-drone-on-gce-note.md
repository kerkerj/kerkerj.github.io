---
title: "Drone CI on GCE 筆記"
date: 2018-06-29
slug: "drone-on-gce-note"
categories: ['DevOps', 'Docker']
tags: ['Drone', 'CI/CD', 'GCE', 'Docker', 'Docker Compose', 'GitHub']
description: "記錄在 GCE 上使用 Docker 部署 Drone CI 的流程，包含機器建立、Docker 安裝、docker-compose 設定及 GitHub OAuth 整合。"
draft: true
---

1. 先開一台 micro 機器，設定都維持為預設
2. 開好後 login （以下會用 gclound command login）

```shell
# login
$ gcloud compute --project "prebuild-88" ssh --zone "asia-east1-b" "drone-test"

# https://docs.docker.com/install/linux/docker-ce/debian/#prerequisites
# 先確認沒有舊版 docker 存在
$ sudo apt-get remove docker docker-engine docker.io

# update
$ sudo apt-get update && sudo apt-get upgrade

# install
$ sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common

# Add Docker's official GPG key
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88

# setup repository
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

# update again
$ sudo apt-get update

# install docker-ce
$ sudo apt-get install docker-ce

# verify
$ sudo docker run hello-world
```

3. 安裝 docker-compose

```shell
$ sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
```







DRONE_HOST=http://104.155.228.70 DRONE_SECRET=YOUR_DRONE_SECRET DRONE_GITHUB_CLIENT=YOUR_GITHUB_CLIENT DRONE_GITHUB_SECRET=YOUR_GITHUB_SECRET
