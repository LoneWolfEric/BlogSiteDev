---
title: "《计算机网络与通信》学习总结"
date: 2018-08-1
categories:  Course
tag: 
	- 学习
	- 网络
	- 总结
thumbnail: /img/pexels/adult-business-computers-256219.jpg
toc: true
---

自动化专业的计网给我的感觉是一种科普性质的课程，对于层次概念的要求比较高，对于实践不及计算机专业。（计算机专业要求他们根据所学知识设计底层的通信程序）

# 绪论

网络的核心是由路由器构成的网，网络的边缘才是我们这些上网的终端。

网络通信里面有个很好玩的两军问题，这个思想实验说明了在不可靠通信链路上面试图通过通信达到一致是非常困难的。

## 数据包传输方式

- 电路交换：连接实际的电路，传输信息

![捕获](Computer-network-learning-summary\捕获.PNG)

- 分组交换：将大的数据包拆分成较小的部分，加上报文头部组成报文。

![捕获1](Computer-network-learning-summary\捕获1.PNG)

![捕获2](Computer-network-learning-summary\捕获2.PNG)

## 延迟

网络的四种延迟：

- nodal processing delay
- queueing delay（玩游戏的时候卡主要就是他）
- transmission delay
- propagation delay

![延迟](Computer-network-learning-summary\延迟.PNG)

## 五层网络模型

![五层网络模型](Computer-network-learning-summary\五层网络模型.PNG)

# 应用层

## HTTP

喜闻乐见的网页服务

- stateless：服务器不保留任何访问过的请求信息，cookie才保留访问请求信息。chrome F12 network就可以看到server向我们要了那些cookie数据。

## FTP

传输文件

## E-mail

- SMTP

  主要用于发邮件，因为这个协议需要两方同时在场，如果那他来收邮件就会出现不登陆没法收邮件的情况。

- POP3

  用于收邮件，可以下载邮件，但是是单向的，客户端操作无法对服务器端产生影响。

- IMAP

  相比POP3增加了双向通道，客户端操作可以同步到服务器端。
## DNS

域名解析，将网址转化为IP地址。

![DNS](Computer-network-learning-summary\DNS.PNG)

# 传输层

## TCP & UDP

大名鼎鼎的TCP/IP

TCP面向连接，可靠的数据传输，但是因此传输速度远不及UDP。

UDP是尽力而为的传输方式，有数据包丢失，但是速度快，用于容忍数据包丢失的场合。

## RDT

解释了TCP是如何一步步来的。从完全可靠的数据传输链路出发，一步步认为每一个地方都具有可能是不可靠的。最后形成了最终的模型。

![图片1](Computer-network-learning-summary\图片1.png)

# 网络层

## IP

主要就是子网划分：一般的ip都是定死的8的整数倍是网络号，剩下的都是主机号，这样想要组一个子网就会造成ip地址的大量浪费，于时出现子网分割来让我们的网络更加合理地应用。



## Routing algorithms

- Link State

全局感知，算法有点动态规划的感觉。（好像就是动态规划）

![LS](Computer-network-learning-summary\LS.PNG)

- Distance Vector
局部感知，只知道和自己相连的路由器的情况，每次算路径通过与自己相连的路由器交换路由表来获取最短路径。

![DV](Computer-network-learning-summary\DV.PNG)

# 链路层 & 物理层

网路层用IP地址来表示一个终端的身份，而链路层用的是MAC地址。

- CSMA/CD (Collision Detection，冲突检测)

- 交换机 & 集线器：交换机能分割广播域，而集线器只是单纯地将总线相连。

# 其他

学了计网之后了解了IP到底是啥？为啥要装路由器？还有就是知道了WLAN和WIFI的区别，一个是虚拟局域网，一个是实现它的协议。