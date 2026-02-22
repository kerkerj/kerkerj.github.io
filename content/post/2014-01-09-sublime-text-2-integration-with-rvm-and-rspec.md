---
title: '[Sublime Text 2] Integration With RVM and Rspec'
description: "Ruby: /Library/Application Support/Sublime Text 2/Packages/Ruby/Ruby.sublime-build json { env:{ PATH:${HOME}/."
date: 2014-01-09
categories: ['Ruby', 'macOS']
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
