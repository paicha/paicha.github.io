---
layout: post
title: 利用 Dropbox 自动备份 VPS 数据
tags:
  - Dropbox
  - Linux
  - VPS
id: 1378
categories:
  - 折腾不止
date: 2013-07-18 12:46:18 +0800
---

论坛和PT站都运行在 VPS 上，所以备份数据是个问题。前几个月还发生了一次数据丢失的情况，后面才付费找回了数据。经过这次之后，每个星期我都手动备份数据库和网站文件，但老是忘记，这样的方法太笨了。后来在 [V2EX](http://www.v2ex.com/) 看到有人提到了用 [Dropbox](https://www.dropbox.com/) 备份 VPS 数据，于是就花了点时间折腾这个数据备份的问题。

<!--more-->

在服务器安装 Dropbox：

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

    ~/.dropbox-dist/dropboxd # 打开网址绑定Dropbox

    # 绑定后同步文件夹默认为 /root/Dropbox/
    # 增加其他目录只要 ln -s 软连接到 root/Dropbox/即可

    wget https://www.dropbox.com/download?dl=packages/dropbox.py

    chmod 700 dropbox.py

    dropbox.py start # 启动 Dropbox

    dropbox.py status # 查看状态


备份数据库脚本：

    #!/bin/bash

    # 获取当前星期
    DATE=`env LANG=en_US.UTF-8 date +%A`

    # 备份数据库，压缩数据库到同步文件夹（自动覆盖上周文件）
    /usr/local/mysql/bin/mysqldump -u username -ppassword dbname > /path/to/dropbox/mysql_backup_${DATE}.sql

    # 启动 Dropbox 同步
    /root/dropbox.py start


备份网站文件脚本：

    #!/bin/bash

    # 获取当前时间
    DATE=`env LANG=en_US.UTF-8 date +%Y-%m-%d--%H-%M`

    # 压缩打包网站文件到同步文件夹
    tar -zcpf /path/to/dropbox/host_backup_${DATE}.tar.gz /path/to/website/

    # 删除文件夹中30天前的备份文件
    find /path/to/dropbox/ -mtime +30 -name "*.gz" -exec rm -rf {} \;

    # 启动 Dropbox 同步
    /root/dropbox.py start


计划任务：

    # 每天凌晨1点备份数据库
    0 1 * * * /root/mysql_backup.sh

    # 每周六凌晨3点备份网站文件
    0 3 * * 6 /root/hosts_backup.sh

    # 每天早上7点检测 Dropbox 是否空闲，是则退出 Dropbox
    0 7 * * * /root/dropbox.py status |grep -i "idle" &&  /root/dropbox.py stop


速度问题：
~~由于 VPS 在国内，所以上传速度不太给力，始终在 100kb/s 以下，暂时找不到什么好的加速方法。慢点也照样用了，反正都是自动备份。~~美国 VPS 可以达到 500kb/s 以上的速度。

占用资源：
上传的时候 Dropbox 大概占用 30MB 内存左右，所以如果你的服务器内存宽裕的话，长期运行也没关系。

最后，欢迎点击 [Dropbox 邀请链接](http://db.tt/FKLEjNyX)，注册安装客户端后，你我都可以增加 500MB 的空间。
