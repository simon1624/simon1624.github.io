---
layout: post
title:   在Ubuntu系统中使用Windows软件
author: Simon
tags: 
- ubuntu
---



# 在Ubuntu系统中使用Windows软件

​	在将电脑系统转到`ubuntu`系统上后遇到的第一个问题就是无法使用**微信**和**qq**，此外还有很多别的软件不支持`Linux`。因此，最好的解决方法就是直接在`ubuntu`系统中安装`Windows`的**`.exe`**文件。（本文中使用**来指代安装的程序名）

* 首先需要安装**`wine`**,在官网有[安装教程](https://wiki.winehq.org/Ubuntu)，对于`Ubuntu18.04`，需要额外安装一个**`FAudio`**包。通过以下命令安装

    ```bash
    sudo add-apt-repository ppa:cybermax-dexter/sdl2-backport
    sudo apt install --install-recommends winehq-stable
    ```

* 下载对应的**`.exe`**安装包，通过以下命令安装

    ```bash
    wine **.exe
    ```

* 安装时会出现很多依赖问题，因为`wine`并没有把整个`windows`都安装了，所以会缺少一些系统库。一个解决办法就是把需要的库安装上，但是，可能会有新的依赖问题，而且即使安装上，微信还是容易崩溃，而且无法使用小程序。找到的新的方法就是通过**`lustris`**安装。

* **`lustris`**是一个用来在`Linux`上安装`Windows`游戏的平台，使用以下命令来安装

    ```bash
    sudo add-apt-repository ppa:lutris-team/lutris
    sudo apt update
    sudo apt install lutris
    ```

* 通过`lutris`安装游戏，以**炉石传说**为例，在搜索栏搜索**炉石传说**，搜不到，只能通过**`hearthstone`**搜索到国际版。在安装国际版时，发现`lutris`也是调用的`wine`来安装`.exe`文件，只是调用的不是默认的**`\usr\local\bin\wine`**而是**`~/.local/share/lutris/runners/wine/lutris-fshack-6.14-3-x86_64/bin/wine`**，也就是说是`lutris`自己安装了一个新的版本的`wine`

* 因此，可以通过以下指令安装`.exe`软件

    ```bash
    ~/.local/share/lutris/runners/wine/lutris-fshack-6.14-3-x86_64/bin/wine **.exe
    ```

* 安装完成后，程序将被安装在**`~\.wine\drive_c\Program Files\**\ `**中，在`lutris`界面左上角有个加号，可以添加本机安装的“游戏”，打开程序的安装路径将`**.exe`加入`游戏启动程序`，设置“游戏“名称，之后就可以`lutris`的**游戏列表**中看到我们安装的发“游戏”了。如图：![lutris截图]({{ site.url }}/images/lutri界面.png)

