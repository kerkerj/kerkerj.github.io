---
title: '[PHP] json_decode error by wrong json string with quotes '
description: "PHP json_decode fails when the JSON string from $_POST has added backslashes. The root cause is the magic_quotes setting in php.ini — disable it to fix the issue."
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
