---
layout: post
title: install a proxy server
tags: [ubuntu, shadowsocks]
toc: true
---



2021年08月05日 
------

- **第一步**,获取云服务器（vps），通过**Github学生优惠**可以白嫖**Digitalocean**的服务器

- **第二步**，安装ssh， 通过**ssh-keygen**生成公钥，创建vps并添加公钥

- **第三步**，通过

    ```bash
    ssh -o StrictHostKeyChecking=no {用户名}@{IP地址}
    ```
    指令登录远程服务器，安装**shadowsocks**，在**/etc**文件夹下创建一个**{代理配置文件名}.json**，填入如下
    
    ```json
    {
    "server":"::",		#"::"表示IPV6
    "server_port":8388,	#VPS端口
    "local_address": "127.0.0.1",	#本地IP
    "local_port": 1080,		#本地端口
    "password":"simon16241030",	
    "timeout":600,
    "method":"aes-256-cfb"
    }
    ```
    
- 第四步，在本地安装__shadowsocks__，如上配置__.json__文件，
  
    ```bash
    {
    "server":"2604:a880:800:10::26:1",#云服务器Ip
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "password":密码,
    "timeout":600,
    "method":"aes-256-cfb"
    }
    ```
    使用`sslocal -c /etc/{代理配置文件名}.json -d start`指令连接

- 第五步，_PAC_连接，使用命令：

    ```bash
    $ genpac --proxy="SOCKS5 127.0.0.1:1080" --gfwlist-proxy="SOCKS5 127.0.0.1:1080" -o autoproxy.pac 
    ```
再生成 __~/auto.pac__，在网络设置，代理栏中填入__file:///home/{用户名}/auto.pac__，在火绒浏览器的网络设置中选择系统代理。

2021年08月06日 
------

- 第六步，开机自启动
  在ubuntu18.04中，没有__rc.local__文件，开机自启动采用__systemd__服务,执行`ls /lib/systemd/system` 可以看到有很多启动脚本，其中就有我们需要的 __rc.local.service__，在__/lib/systmd/system/rc.local.service__文件末尾加入：
  
    ```bash
    [Install]
    WantedBy=multi-user.target
    Alias=rc-local.service
    ```
    __[install]__下的信息提供如何开机启动的信息，__Alias__设置了别名。然后创建__/etc/rc.local__文件，添加脚本信息
    ```bash
    #!/bin/sh 
    echo "this is a test " >> /home/simon/test.txt
    ```
    其中`#!`用于添加脚本解释器，然后需要用指令`$ chmod +x /etc/rc.local` 赋予文件可执行权限
    开机时__systemd__ 默认读取 __/etc/systemd/system__ 下的配置文件，该目录下的文件会链接__/lib/systemd/system__/下的文件，所以需要建立一个软连接
  
    ```bash
    $ ln -s /lib/systemd/system/rc.local.service /etc/systemd/system/
    ```
    然后通过`$ systemclt start rc-local.service` 测试
    `$ systemclt stattus rc-local.service`可以查看服务启动的状态
    `$ journalctl -xe`可以查看问题有关的日志，（-r 反序查看）
    `$ systemctl enable rc-local.service`将服务加入启动项


- 第七步，终端代理，待续~~~~

