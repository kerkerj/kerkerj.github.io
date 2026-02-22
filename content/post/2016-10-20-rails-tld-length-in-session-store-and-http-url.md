---
categories: ['Rails']
date: 2016-10-20T10:47:58+08:00
draft: false
tags: ["rails", "session","url"]
title: tld_length 在 Rails session_store 與 HTTP URL 的設定
description: "Rails session_store 和 ActionDispatch::Http::URL 中 tld_length 的設定差異與實作原理。"
---


工作上 rails 在每個 stage 的 domain 長度都不太一樣

例如 production 是 `example.com`，staging 是 `kerkerj.staging.example.com`

```ruby
MyApp::Application.config.session_store :redis_session_store, {
  key: 'example_session_token',
  domain: :all,
  tld_length: 4,
  serializer: :hybrid,
  redis: {
    host: "....",
    key_prefix: "...",
    expire_after: 7.day,
  }
}
```

相關原始碼: [action_dispatch/middleware/cookies.rb](https://github.com/rails/rails/blob/ee30661a3cf93ebf0f99905434f4989344549820/actionpack/lib/action_dispatch/middleware/cookies.rb#L283-L299)

在這裡的 `tld_length` 就是看你 domain 的 tld 想設定到哪就寫多少

以 `kerkerj.staging.example.com` 為例，想要 `example.com` 就是 2，想要 `kerkerj.staging.example.com` 就是 4

---

而在 Rails App 裡，在 `config.action_dispatch.tld_length` (或 `ActionDispatch::Http::URL.tld_length` ) 設定的 `tld_length`

在 [rails api document](http://api.rubyonrails.org/classes/ActionDispatch/Http/URL.html#method-i-domain) 的 `#domain` 有用法

原始碼則解釋得更清楚 [actionpack/lib/action_dispatch/http/url.rb](https://github.com/rails/rails/blob/master/actionpack/lib/action_dispatch/http/url.rb#L38-L43)

```
> ActionDispatch::Http::URL.extract_domain("kerkerj.staging.example.com", 1)
=> "example.com"
> ActionDispatch::Http::URL.extract_domain("kerkerj.staging.example.com", 2)
=> "staging.example.com"
> ActionDispatch::Http::URL.extract_domain("kerkerj.staging.example.com", 3)
=> "kerkerj.staging.example.com"
> ActionDispatch::Http::URL.extract_domain("example.com", 1)
=> "example.com"
> ActionDispatch::Http::URL.extract_domain("example.com", 3)
=> "example.com"
```

`tld_length` 在兩邊的實作與意義上不同，需要注意。