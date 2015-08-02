title: 关于 Ubuntu 亮度调节和显卡的问题
tags:
  - Linux
  - Ubuntu
  - 显卡
id: 864
categories:
  - 折腾不止
date: 2012-03-11 14:04:52
---

之前在家[安装设置](http://paicha.me/2012/01/19/572 "Ubuntu 11.10 安装与设置手记")好了 Ubuntu 的时候就发现了屏幕亮度不能调节、独立显卡耗电的问题。可是在学校 Ubuntu 连上不了校园网，于是就把问题搁置了。直到上星期找到了一个 Linux 版本的[中兴客户端](http://paicha.me/2012/03/07/793 "ZTE中兴客户端 for Windows / Linux")连上了校园网，所以就尝试能不能把之前的问题解决。

<!--more-->

Ubuntu 版本是11.10，笔记本电脑是宏碁4750G-2312G75Mnbb，CUP：intel I3-2312M，显卡：NVIDIA GeForce GT 540M，内存：2G+4G。

**亮度无法调节的问题：**

我安装好 Ubuntu 后，它的屏幕亮度是默认是50%的，一般室内情况下25%到30%的亮度已经足够。屏幕太亮了，不但缩短屏幕寿命，眼睛看久了也不舒服。在右上角，系统设置——屏幕，调节亮度条。怎么调都是没反应，但是在那里把亮度调低了之后重启，屏幕亮度是变暗了的。不过这个亮度却保存不了，再重启的话，亮度又回到了50%了。总不能每次调了亮度都重启吧。最后 Google 到了一个方法：

修改 `/etc/default/grub` 文件，打开终端运行：

    sudo gedit /etc/default/grub

添加下面代码到打开的文件里：

    acpi_osi=Linux acpi_backlight=vendor
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi=Linux acpi_backlight=vendor"

更新gurb，终端运行：

    update-grub

重启后就可以直接拖动亮度条或快捷键动态调节亮度了。不过下一次开机还是恢复50%亮度，每次开机都需要自己调节。保存亮度这个问题还没解决，先凑合用着，起码不用再忍受刺眼的亮度了。

**独立显卡耗电问题：**

我的笔记本显卡支持 NVIDIA Optimus 双显卡智能切换技术，这在 Win7 下是非常好用的，根据需要自动切换显卡，功耗与性能兼得。但是在 Ubuntu 下却不能支持 NVIDIA Optimus ，导致独立显卡一直在启用，显卡发热升温，风扇狂转。平时笔记本电池在 Win7 能使用4小时，到了 Ubuntu 两个小时左右就耗光了。Ubuntu 下我根本就用不着独立显卡，所以要想办法把独立显卡禁用。

为了禁用独立显卡这个问题，在 Google 找了很久：

[禁用NVIDIA Optimus的方法](http://forum.ubuntu.org.cn/viewtopic.php?f=42&amp;t=328969)

[双显卡禁用独显完美解决方案](http://forum.ubuntu.org.cn/viewtopic.php?f=169&amp;t=306694)

[双显卡笔记本ubuntu下禁用独显](http://zhyu.me/linux/ubuntu-disabled-independent-graphics-card.html)

[安装 Bumblebee 支持 optimus ](http://forum.ubuntu.org.cn/viewtopic.php?f=42&amp;t=332796&amp;start=0)

[在Ubuntu中禁用独显只用集显](http://forum.ubuntu.org.cn/viewtopic.php?f=77&amp;t=366609)

看了N个帖子，试了几种方法都还没搞定。最后无奈采取了最原始的办法：进入 BIOS 把独立显卡禁用了，这样进入 Win7 的话也只能用集显了。我很少玩游戏，所以就凑合用着吧。

迟点有空再把保存亮度、禁用显卡问题一并彻底解决，到时侯再把过程和方法更新到本文。

弄了一早上，两个问题都没有完美解决。不过就是要这种自己探索解决问题的精神，虽然很多东西都不懂，但是可以[自己学](http://paicha.me/2012/02/23/746 "所谓大学就是——大不了自己学")嘛。

**====================更新分割线====================**

**亮度保存**

看了几个帖子，发现没有完美的解决办法。貌似 Ubuntu 这个亮度是没办法保存的，后来找到了另外一个[方法](http://askubuntu.com/questions/84937/how-to-make-unity-remember-brightness-settings)：让系统启动的时候自动运行脚本，把屏幕亮度调节到预设的亮度。

保存脚本为 `setBrightness.py`

    import dbus
    bus = dbus.SessionBus()

    proxy = bus.get_object('org.gnome.SettingsDaemon','/org/gnome/SettingsDaemon/Power')

    iface=dbus.Interface(proxy,dbus_interface='org.gnome.SettingsDaemon.Power.Screen')

    iface.SetPercentage(30)

其中的最后一行的30就是30%亮度的意思，你可以修改到合适的数值，保存后添加为启动程序：

    python 你的文件存放路径/setBrightness.py


**禁用独立显卡**

最后还是通过[安装 Bumblebee ](http://forum.ubuntu.org.cn/viewtopic.php?f=42&amp;t=332796&amp;start=0)关闭独立显卡解决了耗电问题。之前使用这个方法的时候，一直卡在这个地方过不去：

    【警告】：下列软件包不能通过验证！
    dkms screen-resolution-extra

后来[误打误撞](http://forum.ubuntu.com.cn/viewtopic.php?f=42&amp;t=363075&amp;p=2651438)以 Nvidia私有驱动安装了 bumblebee：

    sudo apt-get install bumblebee bumblebee-nvidia

然后再按照教程重新开始，到了选择笔记本型号的步骤，选择自己的笔记本型号和系统版本。下一步，选择第4个默认选项。重启笔记本。

重启 Ubuntu 后，发现散热口的风明显凉了。拔下适配器，查看功耗：

    grep rate /proc/acpi/battery/BAT0/state

从之前的 2300mA 降低到了 1600mA 左右。如果需要独立显卡运行程序的时候，可以用以下命令：

    optirun 程序名

折腾了不少时间，到这里两个问题都解决了。存个备忘，也给大家一些参考。
