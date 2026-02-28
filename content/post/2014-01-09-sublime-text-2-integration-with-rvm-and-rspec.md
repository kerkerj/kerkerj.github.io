---
title: '[Sublime Text 2] Integration With RVM and Rspec'
description: "Sublime Text 2 整合 RVM 的設定方法，修改 Ruby.sublime-build 改用 rvm-auto-ruby 執行，讓編輯器正確使用 RVM 管理的 Ruby 版本。"
date: 2014-01-09
slug: sublime-text-2-integration-with-rvm-and-rspec
tags: ['ruby', 'macos']
---


Ruby:

`~/Library/Application\ Support/Sublime\ Text\ 2/Packages/Ruby/Ruby.sublime-build`

```json
{
  "env":{
      "PATH":"${HOME}/.rvm/bin:${PATH}"
  },
  "cmd": ["rvm-auto-ruby", "$file"],
  "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
  "selector": "source.ruby"
}
```

[http://rubenlaguna.com/wp/2012/12/07/sublime-text-2-rvm-rspec-take-2/](http://rubenlaguna.com/wp/2012/12/07/sublime-text-2-rvm-rspec-take-2/)
