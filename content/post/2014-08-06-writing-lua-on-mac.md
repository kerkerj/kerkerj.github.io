---
title: "Writing Lua on Mac"
description: "Getting started with Lua on Mac: install via Homebrew and learn basic Lua syntax with examples including Hello World and a recursive factorial function."
date: 2014-08-06
slug: writing-lua-on-mac
tags: ['macos', 'lua']
---


## Install lua on mac

I'm not sure that whether Lua is built on mac originally. 

<span style="color:red;">(Ok, tested on Mac OSX 10.9, there is Lua in it.)</span>

So I installed Lua via [Homebrew](http://brew.sh/).

Install homebrew (optional)

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

Install Lua by homebrew

```
brew install lua
```

## Writing Lua

You can use command: `lua` to interact with lua. (just like `php -a` or `irb`)

```lua
print("Hello World")

```

```lua
function fact(n)
	if n == 0 then
		return 1
	else
		return n * fact(n-1)
	end
end

print("enter a number:")
num = io.read("*number") 
print(fact(num))
		
```

