---
title: '[Rails] PUT v.s. PATCH'
description: "The difference between PUT and PATCH in Rails. Rails 4 switched from PUT to PATCH for resource updates to follow proper HTTP semantics, where PUT means full replacement and PATCH means partial update."
date: 2014-01-02
slug: rails-put-vs-patch
tags: ['rails']
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
