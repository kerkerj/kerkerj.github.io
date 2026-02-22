---
title: '[PHP] json_decode error by wrong json string with quotes '
description: "PHP 從 $_POST 接收的 JSON 字串被加上反斜線導致 json_decode 失敗，根本原因是 php.ini 的 magic_quotes 設定，關閉即可解決。"
date: 2014-01-28
categories: ['PHP', 'JavaScript']
---


Normal json string:

```
{"Hey":"There"}
```

Error json string got from $_POST variable:

```
{\"Hey\":\"There\"}
```

This may causes by `magic_quotes` in `php.ini`
Magic is never good in development.
