---
title: "Event-driven I/O models and Coroutine Notes"
description: "Event-driven I/O 模型與 Coroutine 的學習筆記，比較阻塞/非阻塞網路模型，整理 Ruby、Python、Go 各語言相關框架，並探討 Reactor 模式的優缺點。"
date: 2014-08-18
categories: ['Python', 'Node.js', 'Ruby']
---


## Event-driven I/O model

首先，聽到 Event-driven 是從 [Node.js](http://nodejs.org/) 得知，

>Node.js® is a platform built on [Chrome's JavaScript runtime](http://code.google.com/p/v8/) for easily building fast, scalable network applications. 

>Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

其實剛聽到這詞會有點陌生，我們可以先從 Victor 所寫的文章開始讀起:
[淺談 coroutine 與 gevent](http://blog.ez2learn.com/2010/07/17/talk-about-coroutine-and-gevent/)

裡面提到了幾種網路模型：
>
1. 阻塞式單一行程  
2. 阻塞式多行程
3. 阻塞式多行程多執行序
4. 非阻塞式事件驅動
5. 非阻塞式 coroutine 


以下是各語言 event-driven 的 model  
(Node.js 就不列出了，本身就是 event-driven 設計)

```
Event-driven model:
* Ruby 	-> [Eventmachine](http://rubyeventmachine.com/)  
			-> [Celluloid::IO](https://github.com/celluloid/celluloid-io)   
			-> [Thin Server](http://code.macournoyer.com/thin)   
* Python 	-> [Twisted](https://twistedmatrix.com/trac/)  
			-> [Tornado](http://www.tornadoweb.org/en/stable)   
* Perl	 -> [Perl Object Environment (POE)](http://poe.perl.org)    
* PHP	 	-> [REACT](http://reactphp.org) 
``` 


以 Ruby 建構 API Service 的話，可以使用 Sinatra + Eventmachine + thin proxy + Nginx 的 Solution:  
[Embedding Sinatra within EventMachine](http://recipes.sinatrarb.com/p/embed/event-machine)

延伸閱讀:   
[2008 - Eventmachine and Reactor pattern](https://www.igvita.com/2008/05/27/ruby-eventmachine-the-speed-demon/)   
[2012 - Lessons Learnt From Building a REST API](http://developwithstyle.com/articles/2012/05/23/lessons-learnt-from-building-a-rest-based-api/)  
[年份未知 - 針對各 framework 進行評測 Event Driven I/O Web Application Server Analysis](https://docs.google.com/document/d/1dU-juYN25FMXdp6Ju62KAIT_0tuuZAPEgZkj-aT6kPQ/edit#heading=h.lafes4uxj2b0)  
[Twisted 教程](http://blog.sina.com.cn/s/blog_704b6af70100py9n.html)  

#### Reactor 模型
Node.js 處理 concurrency 是 Reactor mode  
Ruby 的 Goliath framework 也是 Reactor mode  
Golang, Erlang 是 CSP (communicating sequential process)

現在比較流行的是 event-driven 的 Reactor mode, e.g. Node.js, Goliath  
但是 Node.js 比較令人詬病的是 code 難維護，太多層層的 callback 會擾亂邏輯  
畢竟線性處理比較符合人類思維   
而 Ruby 使用 Fiber 以避免寫出過多的 callback  
Python 的 Twisted 也已經存在好一陣子，穩定發展中  

不過以 Node.js 典型的應用的確是 proxy, API server  
因此我們可以參考 Node.js 的特性，使用其他語言來達到同樣的效果  
那就是 event-driven  


## Coroutine Programming

```
* Golang - goroutine (native) 
* Python - [gevent](http://www.gevent.org/)  
* Ruby - Fiber (native)   
* Lua   
```

延伸閱讀:  
[Ruby 的 eventmachine 相關討論](http://www.bigfastblog.com/)  
[Ruby Fiber 指南](http://www.blogjava.net/killme2008/archive/2010/03/11/315158.html)  
[2012 - Lua - Coroutine introduction](http://electronic-blue.herokuapp.com/blog/2012/06/coroutine-an-introduction/)  
[2009 - Fibers & Cooperative Scheduling in Ruby](https://www.igvita.com/2009/05/13/fibers-cooperative-scheduling-in-ruby/)  
[2010 - Lua、LuaJIT Coroutine和Ruby Fiber的切換效率對比](http://www.blogjava.net/killme2008/archive/2010/03/02/314264.html)


---
建構 API Service 考慮到的除了 Server 以外，  
語言的特性通常也必須考慮進去，例如  
sync v.s. async  
coroutine v.s. non-coroutine  
但通常以當前需求而言，其實也不需要 over-design  
只要選擇拿手的，能夠快速方便的開發出雛形，  
我想等到真的快要 10000 per/second request 時，再來煩惱更進一步的架構吧
