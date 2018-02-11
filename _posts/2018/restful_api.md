---
layout: default
title: 如何理解RESTful API
---


# 如何理解RESTful API

### 基本概念
常常听人讨论RESTful API，一种API设计风格。那么RESTful API到底传统API有什么不同？为什么RESTful API这么流行？

REST -- REpresentational State Transfer 直接翻译：表现层状态转移。REST描述的是在网络中client和server的一种交互形式。Server提供的RESTful API中，URL中只使用名词来指定资源，原则上不使用动词。“资源”是REST架构或者说整个网络处理的核心。

比如：

	http://api.qc.com/v1/newsfeed: 获取某人的新鲜; 

	http://api.qc.com/v1/friends: 获取某人的好友列表;

用HTTP协议里的动词来实现资源的添加，修改，删除等操作。即通过HTTP动词来实现资源的状态扭转：

	GET    用来获取资源，
	POST  用来新建资源（也可以用于更新资源），
	PUT    用来更新资源，DELETE  用来删除资源。
	
比如：

	DELETE http://api.qc.com/v1/friends: 删除某人的好友 （在http parameter指定好友id）
	POST http://api.qc.com/v1/friends: 添加好友

Server和Client之间传递某资源的一个表现形式，比如用JSON，XML传输文本，或者用JPG，WebP传输图片等。当然还可以压缩HTTP传输时的数据（on-wire data compression）。

用 HTTP Status Code传递Server的状态信息。比如最常用的 200 表示成功，500 表示Server内部错误等。

(怎样用通俗的语言解释REST，以及RESTful？ - 覃超的回答 - 知乎
https://www.zhihu.com/question/28557115/answer/48094438)

### 一句话概括
用URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。

1. 看Url就知道要什么资源
2. 看http method就知道做什么操作
3. 看http status  code就知道结果如何



### 设计思路

REST 是面向资源的，这个概念非常重要，而资源是通过 URI 进行暴露

**为什么会产生REST API**

REST风格的优势是什么？ - 淘李福的回答 - 知乎
https://www.zhihu.com/question/33959971/answer/60280136

> REST的目的是“建立十年内不会过时的软件系统架构"，所以它具备三个特点：
>  
> 1. 状态无关 —— 确保系统的横向拓展能力
> 2. 超文本驱动，Fielding的原话是”hypertext-driven" —— 确保系统的演化能力
> 3. 对 resource 相关的模型建立统一的原语，例如：uri、http的method定义等 —— 确保系统能够接纳多样而又标准的客户端
> 
> 从另外一个角度看，第一条保证服务端演化，第三条保证客户端演化，第二条保证应用本身的演化，这实在是一个极具抽象能力的方案。

**REST和RPC的比较**

WEB开发中，使用JSON-RPC好，还是RESTful API好？ - Vincross的回答 - 知乎
https://www.zhihu.com/question/28570307/answer/163638731

> REST是一种设计风格，它的很多思维方式与RPC是完全冲突的。
> 
> RPC的思想是把本地函数映射到API，也就是说一个API对应的是一个function，我本地有一个getAllUsers，远程也能通过某种约定的协议来调用这个getAllUsers。至于这个协议是Socket、是HTTP还是别的什么并不重要；
RPC中的主体都是动作，是个动词，表示我要做什么。
> 
> REST则不然，它的URL主体是资源，是个名词。而且也仅支持HTTP协议，规定了使用HTTP Method表达本次要做的动作，类型一般也不超过那四五种。这些动作表达了对资源仅有的几种转化方式。



### 提个问题

1. 对于用户登录和用户退出这两个业务需求，REST指导下的架构和设计如何满足
2. 批量的删除、修改、新增如何满足

对于第1个，我的解释是把“登录”作为一个资源，所以登录是POST /logins，退出是DELETE /logins

对于第2个，我倾向于在单个资源的set如（/users是user的set）以上搞一个“set的set”，所以批量操作事实上是对“set的set”的单条操作
