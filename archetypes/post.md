---
title: "{{ replaceRE `^\d{4}-\d{2}-\d{2}-` `` .File.ContentBaseName | replaceRE `-` ` ` | title }}"
date: {{ dateFormat "2006-01-02" .Date }}
slug: "{{ replaceRE `^\d{4}-\d{2}-\d{2}-` `` .File.ContentBaseName }}"
categories: []
tags: []
description: ""
---
