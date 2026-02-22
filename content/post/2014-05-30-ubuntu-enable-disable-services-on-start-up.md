---
title: '[Ubuntu] enable/disable services on start-up'
description: "Ubuntu 使用 update-rc.d 指令設定服務開機自動啟動或停用，以 apache2 為例示範 enable/disable 的用法。"
date: 2014-05-30
categories: ['Linux']
---


Take apache for example: 

Disable - `update-rc.d -f apache2 remove`

Enable - `update-rc.d apache2 defaults`
