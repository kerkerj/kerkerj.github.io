---
title: '[Rails] PUT v.s. PATCH'
description: "Rails 中 PUT 與 PATCH 的差異：Rails 4 改用 PATCH 進行資源更新以符合 HTTP 語意規範，PUT 代表完整替換而非部分更新。"
date: 2014-01-02
categories: ['Rails']
---


According to rails convention,

PUT is used for updating an existing resource

POST is used for creating a new resource

In rails 4, PUT has been changed to PATCH to avoid confusion.

```
     posts GET    /posts(.:format)                            {:action=>"index", :controller=>"posts"}
          POST   /posts(.:format)                            {:action=>"create", :controller=>"posts"}
 new_post GET    /posts/new(.:format)                        {:action=>"new", :controller=>"posts"}
edit_post GET    /posts/:id/edit(.:format)                   {:action=>"edit", :controller=>"posts"}
     post GET    /posts/:id(.:format)                        {:action=>"show", :controller=>"posts"}
          PUT    /posts/:id(.:format)                        {:action=>"update", :controller=>"posts"}
          DELETE /posts/:id(.:format)                        {:action=>"destroy", :controller=>"posts"}

```

延伸閱讀：ihower - [HTTP Verbs: 談 POST, PUT 和 PATCH 的應用](http://ihower.tw/blog/archives/6483)
