---
title: 搭建web部署系统
date: 2022-03-21 10:55:58
tags: 
 - lv2
categories:
  - 程序员的自我修养
---
1. 安装jerkins docker ，准备好代码仓库（github/gitlab）
在自己的机器上跑起来 done
使用jenkins将码云上的git仓库拉到服务器
将代码拉到服务器后，可以手动触发构建、部署
完成自动化部署
开发一个自己的插件，具体做啥还没想好

在开发服务器上既部署了jenkins也部署了项目，通过不同的端口号访问，很不安全也不好。


我将Jenkins部署系统和网站运行在同一台服务器上，但它们使用的不同端口号。部署系统是8008，网站是8009

Jenkins是一款开源的自动化部署工具，可以通过插件来实现各种不同的功能。其中，SSH Remote Hosts插件允许Jenkins在远程服务器上执行命令。


先使用publish over ssh插件将内容发送到服务器上，然后再使用ssh remote hosts配置去远程执行服务器上的命令。
## ssh remote hosts 的作用
该插件允许Jenkins将指令发送到远程服务器，并获取执行结果。这对于自动化部署和持续集成非常有用。通过SSH Remote Hosts插件，您可以配置Jenkins与多个远程服务器之间的连接。您可以指定服务器的IP地址、用户名、密码、端口号等信息。在建立连接后，您可以在Jenkins上执行各种命令，如运行shell脚本、安装软件包、复制文件等等。

在使用SSH Remote Hosts时，需要注意以下几点：

在远程服务器上安装SSH服务，以便Jenkins可以连接到服务器。
确保您在Jenkins服务器上安装了SSH插件。
在SSH Remote Hosts插件的配置中，确保您提供了正确的IP地址、用户名、密码和端口号。
确保您在Jenkins中的作业中使用了正确的SSH Remote Hosts插件的配置。
使用SSH Remote Hosts插件时，需要小心，确保您使用了正确的权限和正确的命令。同时，要定期更改您的密码，以确保安全性。