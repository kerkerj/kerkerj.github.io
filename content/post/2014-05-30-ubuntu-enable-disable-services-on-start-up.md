---
title: '[Ubuntu] enable/disable services on start-up'
description: "Take apache for example: Disable - update-rc.d -f apache2 remove Enable - update-rc.d apache2 defaults"
date: 2014-05-30
categories: ['Linux']
---


Take apache for example: 

Disable - `update-rc.d -f apache2 remove`

Enable - `update-rc.d apache2 defaults`
