title: 如何创建 iOS 的描述文件
date: 2016-01-09 13:57:43
tags:
  - iOS
  - 代理
  - APN
categories:
  - 折腾不止
---

众所周知，iOS 设置代理可以在 Setting - WLAN - HTTP PROXY 进行设置。但是在蜂窝网络环境下，就没办法手动设置代理了。这时候，我们可以创建 iOS 的描述文件来做这件事。

iOS 的描述文件是做什么的呢？实际上这是一个 XML 格式的配置文件，可以配置一些网络设置、安全策略、电子邮件等等，是方便管理者统一配置 iOS 设备的一种方法。

<!--more-->

首先，安装 [Apple Configurator 2](https://itunes.apple.com/us/app/apple-configurator-2/id1037126344) 这个管理工具。菜单栏 File - New Profile，新建一个描述文件。从左侧栏就可以看到，有许多可配置的选项。这里我想配置蜂窝网络的代理，选择 cellular 。如下图编辑后保存到本地。最后可以使用软件导入安装，或者启动 Web 服务器，通过 Safari 打开配置文件的网址进行安装。

![apn-profile](/uploads/apn-profile.png)
