---
title: '[Rails] ActionController::InvalidAuthenticityToken when useing Rails4 as API '
description: "Rails 4 作為 API 使用時出現 InvalidAuthenticityToken 的解法：在 ApplicationController 中將 protect_from_forgery 改為 with: :null_session。"
date: 2014-07-03
slug: rails-actioncontrollerinvalidauthenticitytoken-when-useing-rails-as-api
tags: ['rails']
---


change  
`protect_from_forgery with: :exception`  
to  
`protect_from_forgery with: :null_session`  
in `ApplicationController`  
