---
layout: post
title: Hystrix
date: 2017-03-08 15:30:00
tags:
- Linux
categories: Linux
---

# 服务雪崩效应
分布式系统中某个基础服务不可用，最后导致整个系统都不可用，叫雪崩效应。

# 服务雪崩效应的定义
服务雪崩效应是因为**服务提供者**的不可用，导致**服务调用者**的不可用，并**逐渐放大**的过程。        
![服务雪崩](https://sfault-image.b0.upaiyun.com/417/851/4178518469-578b40005071a_articlex)

# 服务雪崩效应的原因
1. 服务提供者不可用
2. 重试加大流量
3. 服务调用者不可用

#### 服务提供者不可用
* 硬件故障
* 网络原因
* 程序bug
* 缓存击穿
* 用户大量请求

解决方式：
* 如果是应用重启后，导致缓存失效，可以做缓存预热
* 如果是用户大量请求，可以做好监控，监控到请求的增长，并提前添加机器。也可以做到自动扩容

### 重试加大流量
* 用户重试
* 代码逻辑重试

解决办法：
* 采用加载动画，提高用户的忍耐等待时间
* 提交按钮添加强制等待时间

### 服务调用者不可用
* 同步等待，造成资源耗尽，比如线程。
当服务调用者使用同步调用时，会产生大量的等待线程占用系统资源。一旦线程资源被耗尽，服务调用者提供的服务也将处于不可用状态。于是雪崩效应就产生了。

解决办法：
* 资源隔离，对调用服务的线程进行隔离
* 不可用服务的调用快速失败。有超时机制，熔断器和降级实现。


# 使用Hystrix预防服务雪崩
Hystrix [hɪst'rɪks]的中文含义是豪猪, 因其背上长满了刺,而拥有自我保护能力. Netflix的 Hystrix 是一个帮助解决分布式系统交互时超时处理和容错的类库, 它同样拥有保护系统的能力.

Hystrix的设计包括：
* 资源隔离
* 熔断器
* 命令模式


### 隔离
给每个依赖的服务，分配隔离的线程。防止一个服务耗尽所有线程。        
比如，一个商品详情页面会依赖商品服务，价格服务，评论服务。给每个服务分配不同的线程，这样，即使评论服务不可用，也不会影响别的服务。        

### 熔断器模式
服务健康状态 = 请求成功数 / 请求总数
熔断器开关的状态是根据当前服务健康状态和设定的阈值比较决定的。        
1. 当开关关闭时，请求被允许通过。如果当前健康状况高于设定阈值，开关继续保持关闭，如果当前健康状况低于设定阈值，开关切换为打开状态。
2. 当熔断器开关打开时，请求被禁止通过。
3. 当熔断器开关处于打开状态，经过一段时间后，熔断器会自动进入半打开状态，这时熔断器只允许一个请求通过。当该请求调用成功时，熔断器恢复到关闭状态。弱请求失败，继续保持打开状态，接下来的请求禁止通过。再经过一段时间后，重复前面的动作，进入半打开状态。

熔断器的开关能保证服务调用者在调用异常服务时，快速返回结果，避免大量的同步等待。并且熔断器在一段时间后继续检测请求执行结果，提供恢复服务调用的可能。




