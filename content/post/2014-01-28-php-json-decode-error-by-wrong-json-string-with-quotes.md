---
title: '[PHP] json_decode error by wrong json string with quotes '
description: "Normal json string: {Hey:There} Error json string got from $POST variable: {Hey:There} This may causes by magicquotes..."
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
