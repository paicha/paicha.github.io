title: Ubuntu 11.10 安装与设置手记
tags:
  - Linux
  - Ubuntu
  - 教程
id: 572
categories:
  - 折腾不止
date: 2012-01-19 19:20:41
---

用了电脑这么多年，一直安装的都是Windows系统。呃，准确点说是盗版 Windows，现在换了笔记本电脑才用上了正版 Windows 7。最近因为学习的需要，同时也体验一下微软以外的系统，所以就投入了 Ubuntu 的怀抱。开源的系统不像Windows那么容易上手，很多问题都要自己动手找解决办法。断断续续折腾了几天才把 Ubuntu 安装设置好了，以下是我安装与设置 Ubuntu 的过程，记录下来分享。也欢迎你一起來探索 Linux。

<!--more-->

安装的版本是11.10，笔记本电脑是宏碁4750G-2312G75Mnbb，CUP：intel I3-2312M，显卡：NVIDIA GeForce GT 540M，内存：2G+4G。因为是新手，所以选择了WUBI安装。这种安装方法很简单，不用分区什么的，安装与卸载都是在 Windows 下进行，就跟安装一个软件那么简单。缺点就是运行效率比直接安装在物理磁盘会低一些。

### **一、Wubi安装系统**

首先去官方网站下载[ Wubi 安装程序](http://www.ubuntu.com/download/ubuntu/windows-installer "WUBI安装程序")，Wubi 安装默认会从网上下载最新版，为了节省下载的时间，我们可以预先下载[ Ubuntu 的 iso 文件](http://www.ubuntu.com/download/ubuntu/download "最新版Ubuntu")，和 Wubi 放在同一个文件夹里，运行 Wubi 就可以安装了。

![](/uploads/wubi-install-1.jpg)

驱动器：建议选择非系统盘。

安装大小：自行选择，建议10G以上。

口令：进入 Ubuntu 系统需要的密码，鉴于安装配置要多次用到密码，在这里可以设置得简单些。

点击安装，完成后会提示重启。

![](/uploads/wubi-install-2.jpg)

重启后，Windows XP 系统的话会显示选择系统的画面，选择 Ubuntu 就行了。而 Win7 会直接进入 Ubuntu。

![](/uploads/wubi-install-3.jpg)

进入了 Ubuntu 系统后，系统会自动安装。这里请注意，如果网速不错的话，推荐联网安装，这时候 Ubuntu 会自动识别硬件，安装一些更新。这会耗费一定时间，如果网速实在不行，断网安装也没关系。我选择了联网更新安装，耗时大约一个多小时。

### **二、系统设置**

Ubuntu 就算是安装好了，但是还不够，还需要设置一下才能更好地使用。

#### **1.更新软件源**

刚安装好的系统此时并不是最新的，我们还需要联网更新。首先选择服务器：点击右上角的齿轮——选择系统设置——选择软件源，选择下载自：主服务器。输入密码，关闭。

![](/uploads/wubi-install-4.jpg)

按下 Ctrl+Alt+T ，打开终端。复制以下命令回车运行。注意要在终端里使用鼠标右键粘贴。
    
    sudo apt-get update && sudo apt-get upgrade

这时候会要求输入密码，注意，在终端里输入密码是不会有任何显示的，也不能回删，要求一次性输入。这时候就会联网下载更新安装，结束后会显示`用户名@ubuntu:~$`这表示命令执行完了。

![](/uploads/wubi-install-5.jpg)

#### **2.安装插件**

在终端复制运行以下命令。

    sudo apt-get install ubuntu-restricted-extras

安装好后就可以支持多媒体播放了。

#### **3.安装语言支持**

打开系统设置——语言支持。这时候会检查语言支持，如果提示没完全安装的话就点击安装。

#### **4.安装显卡驱动**

为了这个我折腾了两天，因为这个显卡驱动是涉及后面开启3D桌面的，我一开始安装驱动却开启不了，而不安装反倒正常。在网上找了一些资料，其实不安装显卡驱动也是可以的。因为聪明的 Ubuntu 已经支持绝大多数的显卡了。你可以运行以下命令，如果每一项都是`yes`，那么你可以选择不安装显卡驱动。

    /usr/lib/nux/unity_support_test -p

![](/uploads/wubi-install-6.jpg)

如果不行的话只有安装驱动了，打开系统设置——附加驱动——选择推荐的驱动——激活。这时候就会下载安装显卡驱动了。安装好后需要重启。

![](/uploads/wubi-install-7.jpg)

#### **5.开启3D桌面**

3D桌面除了真的很酷之外，还是有实用的，旋转立体桌面就是很实际的应用。

![](/uploads/wubi-install-8.jpg)

显卡正常安装好了之后就安装 CCSM 启用 3D 桌面，执行以下命令

    sudo apt-get install compizconfig-settings-manager

安装了之后更改桌面尺寸。按下键盘的徽标，在弹出的面板搜索`CompizConfig`，打开CompizConfig设置管理器，进入常规选项——桌面尺寸，将「水平尺寸」、「垂直尺寸」、「桌面数量」修改为分别4、1、1。后退，点击「桌面」，勾选「旋转立方体」。可能会有些提示，一律忽略、禁用。这时候很可能窗口会崩溃了，别慌。如果CCSM还能动的话，勾选「Ubuntu Unity Plugin」，这时候窗口就回来了。如果还不行，按下`Ctrl+Alt+F1`输入用户名和密码登录。输入以下命令

    killall gnome-session

这时系统会回到登录界面。输入密码登录系统。这时候你按下`Ctrl+Alt+方向键`试试能不能旋转 3D 桌面。行的话就成功开启 3D 桌面了，不行的再重复刚才的设置。CCSM 本身就开启了一部分的 3D 效果，太炫目的也不推荐。关于 3D 桌面更详细的教程可以[参考这里](http://forum.ubuntu.org.cn/viewtopic.php?f=94&amp;t=140531 "3D桌面详细教程")

#### **6.修改字体**

你可能不太喜欢 Ubuntu 默认的字体，可以看看这个[修改字体的教程](http://forum.ubuntu.org.cn/viewtopic.php?f=8&amp;t=348963 "修改字体")。

#### **7.修改窗口按钮位置**

Ubuntu 窗口按钮的默认位置是在左边的，跟 Windows 相反，一时很不习惯。可以通过以下方法修改：打开软件中心，搜索安装配置编辑器。安装好后打开，在左侧目录树中，找到`/apps/metacity/general/`，在右侧找到`button_layout`，修改值为`menu:minimize,maximize,close`，这样窗口按钮就在右边了。

#### **8.修改拼音输入法**

默认是 Ctrl+空格键 打开输入法，你可以点击右上角输入法图标——首选项，这里可以修改快捷键、输入法显示等。如果你要设置模糊音的话，首选项——显示语言栏——选择总是。这时候你在文本框打开输入法的时候就看到有语言栏了，点击里面的感叹号或者齿轮——点击`Fuzzy syllable`就可以设置模糊音了。

#### **9.安装常用软件**

QQ：很遗憾，QQ这个软件虽然有 Linux 版本，但是很简陋。要使用QQ的话建议用[WebQQ](http://web.qq.com/ "WebQQ")。

浏览器：预装的 Firefox 浏览器不错，但我更推荐 chromium。软件中心搜索安装即可

解压缩软件：在软件中心搜索安装 7zip。

办公软件：预装了的 LibreOffice。

图片编辑软件：GIMP，软件中心搜索安装就好了。

软件管理：新立得软件包管理器，软件中心安装。

动态桌面图标 cairo-dock：挺好用的一款软件，但是这个前提要安装了3D桌面。然后打开新立得软件包管理器，点击搜索「cairo-dock」。在列表中选中「cairo-dock 」并选择「标记以便安装」。点击应用，就可以安装了。打开 GLX-Dock（使用OpenGL的Cairo-Dock） 就可以运行了。设置方面摸索一下就清楚了，开机自启动的设置也在里面。

![](/uploads/wubi-install-9.jpg)

### **三、后记**

折腾到这里，整个 Ubuntu 的设置也就差不多了。过程中可能会出现各种各样的问题，善于利用搜索引擎大多可以找到解决方法。另外在这里推荐 [Ubuntu中文论坛](http://forum.ubuntu.org.cn/index.php "Ubuntu中文论坛")，里面有很多关于Ubuntu的教程、知识。

发扬自由探索精神，生命不息，折腾不止。
