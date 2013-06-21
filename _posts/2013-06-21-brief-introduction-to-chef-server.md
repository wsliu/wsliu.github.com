---
title: 初见Chef-Server
layout: post
tags : [chef]
---
在前篇曾经提过用chef配置的三种方式，我们已经试过了最简单的chef-solo，我们前进一步了解chef-server。

####chef-solo vs chef-server
	
* chef-solo比较合适单机的配置，你登陆或者ssh到目标机，执行chef-solo即可。
* chef-server则可以handle更复杂的情况。同事配置、管理多台机器。

（这里介绍的非常简单，如果想要了解的更多，请自行google）
##chef 组成
Chef 包含主要三个要素: 一个server， 一个或多个nodes， 至少一个workstation

![chef composing](http://docs.opscode.com/_images/overview_chef_draft.png)
（图片来源：http://docs.opscode.com/_images/overview_chef_draft.png ）

* Chef server是整个Chef的核心，它定义了相关策略，cookbooks, 还负责注册nodes
* Workstation顾名思义工作台，是你定义相关策略(例如roles, environments, data bags…)和开发cookbooks的地方。完成开发后需要上传至Chef Server
* node则是你要配置的目标机，会通过chef-client来进行自动化配置

在真正开始用chef-server配置前，你要先把chef server和 workstation搭建起来。

##搭建chef server

正所谓条条大路通罗马，搭建chef server也有很多种方法，笔者就在各种方法中游荡，纠结于哪种办法最简单，最fashion， 浪费/花费了不少时间。其实吧，先选一条道走到黑，初学真不适合多想。 First make it work

我选择在Linux系统中安装chef-server。
[这里](http://wiki.opscode.com/display/chef/Installing+Chef+Server+using+Chef+Solo)提供了用chef-solo安装chef-server的方法，但是很不幸的是这里只适用于chef-server 10. 再吐槽一下，网上很多文档随着时间的流逝已经不再准确了，如果你遇到错误了，那不是你的错，别气馁，google吧。

如果你想安装chef-server 11，点[这里](http://www.opscode.com/chef/install/)

##搭建workstation
[这里](http://docs.opscode.com/install_workstation.html)有关于如何搭建workstation的详细介绍，一步一步来吧

本文参考：
http://docs.opscode.com/chef_overview.html
http://wiki.opscode.com/display/chef/Installing+Chef+Server+using+Chef+Solo
