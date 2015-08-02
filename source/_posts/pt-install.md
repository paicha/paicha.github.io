title: 麦田 PT 架设手记
tags:
  - PT
  - Ubuntu
  - 教程
id: 1258
categories:
  - 折腾不止
date: 2012-11-10 22:54:11
---

一直都在忙着弄学校的论坛，在上学期我们论坛就上线了BT共享的应用。主要是通过在学校架设公共的BT服务器，然后在论坛做种发帖来分享资源。这种方法最大的缺点就是缺少 PT 站点的强制做种机制，导致做种人数太少。很多人下载后就撤种，不少资源都只有我一个人做种；再者就是资源的分类、下载列表不够 PT 站点专业和直观，不利于长远的发展。一般大一点的高校都有自己的 PT 站点，有完善的分享机制，更加有利于分享资源。虽然我们学校不大，但如果有几百、上千人的爱好者长期分享的话，还是可以试一试的。哈哈，说不定一不小心，就创造广科的历史了。

<!--more-->

使用的是 ubuntu-12.10-server-amd64 系统，安装系统时选择组件 SSH、LAMP。

**一、准备工作：**

1.  安装好系统后，更新软件包列表和升级软件包

    `sudo apt-get update && sudo apt-get upgrade`

2.  安装 subversion

    `sudo apt-get install subversion`

3.  下载最新版的麦田PT

    `svn checkout http://mtpt.googlecode.com/svn/trunk/ mtpt-read-only`

**二、参考官方Wiki一步一步操作**

[https://code.google.com/p/mtpt/wiki/InstallGuide](https://code.google.com/p/mtpt/wiki/InstallGuide)

**三、架设成功打开 PT 站界面**

![](/uploads/pt.jpg)

**四、其他**

1. Windows 下远程 SSH 连接主机推荐使用 putty 软件。

2.  搭建好了之后，因为麦田 PT 是西北农林科技大学修改的，所以要针对我们的论坛修改一些网站设置。这时候还要搭建一个 FTP 服务器，方便上传和修改后台源文件。

    `sudo apt-get install vsftpd`

    安装后配置 FTP

    `sudo vi /etc/vsftpd.conf`

    设置

        anonymous_enable=NO
        local_enable=YES
        write_enable=YES
        local_umask=022

    `sudo /etc/init.d/vsftpd restart`

    在FTP 软件输入本地用户名密码登陆即可

3.  搭建好 PT 站才是刚刚开始，接下来要慢慢修改一些设置、UI等等的细节了，继续折腾。

4.  额，明天还要继续过光棍节。
