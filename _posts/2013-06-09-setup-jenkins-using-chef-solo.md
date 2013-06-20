---
title: Chef 开篇
layout: post
tags : [chef]
---

以此篇开始由浅入深，逐步介绍chef。先不讲原理，从实践入手。

用不太准确的言语解释chef是什么，它可以把开发以后的事情(运维)自动化起来。局限来看，一个典型的场景：从零开始（裸机），完全自动化生成一套用来编译、测试、打包等的一套环境。用脚本代替一切可能的手工行为。光就这一点就着实吸引人，那我们从一个例子开始吧。
例子开始之前，先吐槽一下windows，它能在各种细节持续地恶心着你。所以，在你还没有很清楚chef的原理，建议利用Linux系统来试验chef的例子。再吐槽一下chef官网的doc，真心不好入门，所以不要再让windows雪上加霜践踏你的学习热情了。

## 准备 
一台Linux系统的机子/虚拟机，这里我用的是ubuntu-12.04.2-server-amd64

## 目标
用chef-solo的方式在目标机上运行我们第一个chef脚本，安装jenkins

###chef-solo简介
Chef 配置及其有三种方式：
Hosted Chef, Chef-Client/Chef-Server和Chef-Solo  
前两种都需要chef server和chef client，区别在于Server是opscode托管还是自己搭建的。  
Chef-solo则不需要Server， 直接在目标机上进行配置。

## 步骤

###1. 安装chef-solo
安装chef-solo的方式有很多种，这里提供其中一种方法。
先安装ruby， 在用ruby安装chef gem包。我这里用的ruby版本是1.9.3

###2. 下载blank chef-repo
这是一个空的chef repository，可以以此为基础建立自己的chef-repo
    
    git clone git://github.com/opscode/chef-repo.git

chef-repo目录介绍

###3. 下载需要的开源的cookbooks
在opscode 的community里有很多开源的cookbook供你使用  
这里我们先用knife命令获取我们所依赖的cookbook: apt和java


	knife cookbook site install apt -o path/cookbooks
	knife cookbook site install java -o path/cookbooks


###4. 写自己的jenkins cookbook
	
	cd path/chef-repo/cookbooks
	mkdir -p jenkins/recipes/
	vi jenkins/recipes/default.rb
	vi jenkins/metadata.rb

default.rb
	
	## Cookbook Name:: jenkins
	# Recipe:: default
	#
	# https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu
	# This is super-simple, compared to the other Chef cookbook I found
	# for Jenkins (https://github.com/fnichol/chef-jenkins).
	#
	# This doesn't include Chef libraries for adding Jenkin's jobs via
	# the command line, but it does get Jenkins up and running.
	#
	# I'd rather build up a Chef cookbook than strip it down, so here's
	# a good starting point.


	include_recipe "apt"
	include_recipe "java"

	apt_repository "jenkins" do
  		uri "http://pkg.jenkins-ci.org/debian"
  		key "http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key"
  		components ["binary/"]
  		action :add
	end

	package "jenkins"

	service "jenkins" do
  		supports [:stop, :start, :restart]
  		action [:start, :enable]
	end
	
metadata.rb
	
	##metadata.rb
	%w(apt java).each {|cb| depends cb}

###5. 配置solo.rb
solo.rb介绍详见[http://docs.opscode.com/config_rb_solo.html](http://docs.opscode.com/config_rb_solo.html)
在运行chef-solo时需要指定一些配置， 用参数-c solo.rb

	##solo.rb
	cookbook_path '/root/workspace/chef-repo/cookbooks'


###6. 运行chef-solo

	chef-solo -c solo.rb -o recipe[jenkins]

###7. 验证jenkins正常运行
打开浏览器访问jenkins地址: http://ipaddress:8080

##小结:
本文避免接触大量新概念而得大头症，用最简单的方法和配置完成chef搭建jenkins
以此为起点，开始对chef有初步的了解

