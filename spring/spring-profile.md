---
layout: post
title: Spring Profile
date: 2017-11-19 20:40:00
tags:
- Java
categories: Java
---



# 前言
由于在项目中使用Maven打包部署的时候，经常由于配置参数过多(比如Nginx服务器的信息、ZooKeeper的信息、数据库连接、Redis服务器地址等)，导致实际现网的配置参数与测试服务器参数混淆，一旦在部署的时候某个参数忘记修改了，那么就必须重新打包部署，这确实让人感到非常头疼。因此就想到使用Spring中的Profile来解决上面描述的问题，并且在此记录一下其使用的方式，如果有不对的地方，请指正!(感谢)。
本文从如下3方面探讨Spring的Profile：

# Spring中的Profile 是什么?
Spring中的Profile功能其实早在Spring 3.1的版本就已经出来，它可以理解为我们在Spring容器中所定义的Bean的逻辑组名称，只有当这些Profile被激活的时候，才会将Profile中所对应的Bean注册到Spring容器中。举个更具体的例子，我们以前所定义的Bean，当Spring容器一启动的时候，就会一股脑的全部加载这些信息完成对Bean的创建；而使用了Profile之后，它会将Bean的定义进行更细粒度的划分，将这些定义的Bean划分为几个不同的组，当Spring容器加载配置信息的时候，首先查找激活的Profile，然后只会去加载被激活的组中所定义的Bean信息，而不被激活的Profile中所定义的Bean定义信息是不会加载用于创建Bean的。

# 为什么要使用Profile
由于我们平时在开发中，通常会出现在开发的时候使用一个开发数据库，测试的时候使用一个测试的数据库，而实际部署的时候需要一个数据库。以前的做法是将这些信息写在一个配置文件中，当我把代码部署到测试的环境中，将配置文件改成测试环境；当测试完成，项目需要部署到现网了，又要将配置信息改成现网的，真的好烦。。。而使用了Profile之后，我们就可以分别定义3个配置文件，一个用于开发、一个用户测试、一个用户生产，其分别对应于3个Profile。当在实际运行的时候，只需给定一个参数来激活对应的Profile即可，那么容器就会只加载激活后的配置文件，这样就可以大大省去我们修改配置信息而带来的烦恼。

