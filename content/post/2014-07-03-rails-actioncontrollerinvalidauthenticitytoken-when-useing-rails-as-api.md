---
title: '[Rails] ActionController::InvalidAuthenticityToken when useing Rails4 as API '
description: "change protectfromforgery with: :exception to protectfromforgery with: :nullsession in ApplicationController"
date: 2014-07-03
categories: ['Rails']
---


change  
`protect_from_forgery with: :exception`  
to  
`protect_from_forgery with: :null_session`  
in `ApplicationController`  
