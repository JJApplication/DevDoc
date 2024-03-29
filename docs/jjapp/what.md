# 设计初衷

和万千的开发者一样， 编码一开始仅仅是作为爱好，因为热爱最终才变为工作

在学习的路上会产生很多想法， 会有很多理论需要用实践去践行，`coding`的魅力就在于此

它可以让我们脑海中的想法， 变为现实， 当我们用自己的双手绘制出一个属于自己想法中的`program`时心中的满足感十足

我的学习之路并非一帆风顺， 我遇到过许多疑问，通过互联网上大家的解答我意识到原来把自己的所思所想写在web上不仅可以用来记录，还可以在某一个时刻帮助到他人

## JJAPP
于是抱着记录生活，记录学习历程的想法，我开始写博客

我开始制作自己的个人网站，希望大家可以通过互联网就直接认识到我是一个怎样的人

随着学习的深入和多元化，渐渐地不再满足于只是简单的写写文章，我有了更多的需求

比如我想我需要：
- 一个在线的相册，用它来记录生活的点点滴滴
- 一个todoList提醒我自己遗漏了什么，需要做什么
- 一个爬虫，爬取一些我感兴趣的站点数据，再用邮件发送给自己
- 一个写随笔的地方，偶尔惆怅时抒发下情绪
- 等等

这个时候我意识到为什么我不写一套属于自己的微服务呢？我想写什么就写什么，不用去研究各种开源程序的使用配置

我可以添加任意自己喜欢的功能，我可以绑定自己喜欢的域名

我可以在这个过程中把平时学习的知识全部应用实践

就这样`JJAPPlication`诞生了

## 设计理念
在最初还是没有这个概念的，只要写一个程序满足自己的功能就够了

到后面我才发现，自己在编码的过程中浪费了很多功夫写重复的代码 。写的代码也零零散散不具备可维护性

于是我为自己的微服务规定了一些开发的规范，并按照这个规范开发后面新的服务

- 一个统一的框架，减少重复的代码，包含最基本的web功能，数据库操作
- 一个统一的数据存储层，不用考虑使用什么存储数据
- 一个统一的通信协议/编码，使用相同格式的报文更易后续的扩展
- 一个统一的微服务管理平台，不用再一个一个通过脚本启动程序
- 等等

## 设计思路
微服务的名字一开始是按照职能来的简单命名法

比如博客就是`Blog`，简历就是`Resume`

后来我想起一些好玩的名字，这些名字既能表达微服务的功能，又让人耳目一新

比如：
- Hermes 取自希腊神话的神与人间的信使， 负责服务消息邮件的发送
- RainbowBridge 取自北欧神话中链接人间和九界的桥梁， 负责整个微服务组的Service Mesh，接管所有的流量
- Sandwich 意为三明治，负责动态端口的微服务网关转发，处理顶层入口和下层微服务间，所以类似三明治的结构

## 后记
`JJAPP`的想法从诞生到维护至今天已经4年多了，这一切的背后都是因为热爱才使这个项目能够不断发展壮大

后面的我可能会转变技术栈，学习其他的东西，但是对于`Coding`的热爱不会改变，也希望`JJAPP`能够发展的更加完善