---
title: 最新科学上网教程(2020年) 
date: 2019-11-01T03:31:23.730Z
tags: ["科学上网"]
categories: ["tools"]
description: 
---

防火墙阻断了大多数异常的tcp连接，因此需要新的方式上网了。目前比较可行的是udp/socket/cdn三种方式。本教程使用udp方式，稍后更新其他方式。

### 准备工作

首选你需要[购买一台在国外的服务器](https://sxy91.com/posts/over-the-wall/)。

然后**登录服务器**  

mac用户直接敲命令`ssh -p 端口 root@ip`

windows用户可以下载工具[xshell](https://www.netsarang.com/zh/free-for-home-school/)来连接到服务器。


**需要的工具**

- shadowsocks（服务端+客户端）
- kcptun（服务端+客户端）

### 一.服务器端安装shadowsocks

1.1 下载并安装ss

```bash
# 安装pip，通过pip安装shadowsocks
pip -V
yum -y install wget
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
pip install shadowsocks
```

1.2配置ss 

```shell
vi /etc/shadowsocks.json
```
用[密码生成器](https://suijimimashengcheng.51240.com/)生成一个密码填写在`password`的地方(自己想一个也行),其余不用改。

```json
{
    "server":"127.0.0.1",
    "server_port":50013,
    "local_port":1080,
    "password":"你的shadowsocks密码",
    "timeout":600,
    "method":"aes-256-cfb"
}
```



1.3启动ss  

```shell
nohup ssserver -c /etc/shadowsocks.json >/dev/null 2>&1 &
```


### 二.服务器端安装kcptun

2.1 下载kcptun

在[kcptun-发布页面](https://github.com/xtaci/kcptun/releases)找到`kcptun-linux-amd64`版本，并复制下载链接。
```shell
# wget +复制的下载链接进行下载。如：
wget https://github.com/xtaci/kcptun/releases/download/v20190924/kcptun-linux-amd64-20190924.tar.gz
# 解压：
tar zxvf kcptun-linux-amd64*.tar.gz
```

2.2 配置kcptun  

```shell
mkdir /etc/kcptun
vi /etc/kcptun/config.json
```

用[密码生成器](https://suijimimashengcheng.51240.com/)生成一个密码填写在`key`的地方(自己想一个也行)，其余不用改。

```json
{
    "target":"127.0.0.1:50013",
    "listen":":4000",
    "key":"你的kcptun密码"
}
```

> `target`里面的`50013`需要和ss的`server_port`对应，若想更改`listen`可填一个1000～65535之间的数字。

> 若服务器开启了防火墙需要打开响应的端口。centos 6 执行:`/sbin/iptables -I INPUT -p udp --dport 4000 -j ACCEPT`。centos 7执行:`firewall-cmd --add-port=4000/udp --permanent`

2.3启动kcptun

```shell
nohup ./server_linux_amd64 -c /etc/kcptun/config.json 1>/dev/null 2>&1 &
```

### 三.安卓手机上网方法

3.1 安装shadowsocks和kcptun插件

到[shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android/releases)页面 下载`shadowsocks--universal.apk
`并安装。

到[kcptun-android](https://github.com/shadowsocks/kcptun-android/releases)页面下载`kcptun--universal.apk`并安装。

3.2 配置ss和插件：

启动shadowsocks点击+号，手动设置。填写如下信息：

```yaml
服务器：你的服务器ip  
远程端口：kcptun.json里配置的`listen`，即4000  
密码：shadowsocks.json里配置的`password`  
```

点击插件-->kcptun-->配置  
清空所有。填写：`key=你的kcptun密码;`  

保存后启动即可。


kcptun需要自启动权限，若提示`无法连接远程服务器：未知插件kcptun`，可以按照这篇文章设置：[华为：无法连接远程服务器:未知插件kcptun](https://blog.csdn.net/cakecc2008/article/details/80182165)

### 四.电脑上网方法

4.1 mac用户

下载ss客户端： 
到[mac-shadowsocks](https://github.com/shadowsocks/ShadowsocksX-NG/releases)页面下载`ShadowsocksX-NG.zip`,解压后拖到应用里启动(已经自带了kcptun客户端，无需再安装)。

启动ss客户端并配置：
![enter description here](https://i.loli.net/2019/11/01/P4NG9fS2OgAr1RM.png)


4.2 windows用户

下载：

windows用户下载[Shadowsocks.zip](https://github.com/shadowsocks/shadowsocks-windows/releases)解压，下载[kcptun-windows.tar.gz
](https://github.com/xtaci/kcptun/releases)解压到Shadowsocks的目录。

启动ss并配置
![enter description here](https://gitee.com/smile365/blogimg/raw/master/sxy91/1574303201500.png)

插件选项留空，插件参数按如下方式填写，替换`你的kcptun密码`就行：

```bash
-l %SS_LOCAL_HOST%:%SS_LOCAL_PORT% -r %SS_REMOTE_HOST%:%SS_REMOTE_PORT% --key 你的kcptun密码
```


若遇到错误：`Shadowsocks 错误: 系统找不到指定的文件`,是因为ss找不到kcptun。需要把需要将插件程序放到你Shadowsocks.exe 所在的目录下。

遇到问题欢迎加微信交流：`smile8365`

相关教程：

- [如何购买一台服务器并配置好](https://sxy91.com/posts/over-the-wall/)
- [如何登录服务器](https://sxy91.com/categories/tools/)


参考连接

- [kcptun使用.json文件启动](https://blog.phpgao.com/kcptun.html/comment-page-1)
- [kcptun使用.sh文件启动](https://home4love.com/3154.html)
- [kcptun](https://github.com/xtaci/kcptun)
- [android-shadowsocks](https://github.com/shadowsocks/shadowsocks-android/releases)
- [kcptun-android](https://github.com/shadowsocks/kcptun-android/releases)
- [mac-shadowsocks](https://github.com/shadowsocks/ShadowsocksX-NG/releases)


