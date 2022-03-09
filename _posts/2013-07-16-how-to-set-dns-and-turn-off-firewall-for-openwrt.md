---
layout: post
title: OpenWrt 路由设置 DNS 与关闭防火墙的方法
tags:
  - DNS
  - OpenWrt
  - 路由器
id: 1370
categories:
  - 折腾不止
date: 2013-07-16 12:28:45 +0800
---

由于 OpenWrt 路由 DNS 转发的问题，查看本地 DNS 配置显示192.168.1.1，可能会造成域名无法解析的情况。默认防火墙的设置也会使得 SSH 链接不上。解决方法如下，留个备忘：
<!--more-->

DNS 设置：

管理界面 - 网络 - 接口 - LAN

![](/assets/openwrt-dhcp.jpg)

防火墙设置：

![](/assets/openwrt-iptable.jpg)
