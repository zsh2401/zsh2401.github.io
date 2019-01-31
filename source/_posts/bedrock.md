---
title: 搭建BedRock服务器的曲折道路
date: 2019-01-26 21:13:49
tags: [c++,电子游戏,Minecraft]
---
{% asset_img Minecraft_logo.png%}
这个寒假,我又想玩Minecraft了...于是决定再开一周目Minecraft服务器,不过与之前不同的是,我决定搭建基岩版服务器[Minecraft Bedrock Sever](https://minecraft.net/en-us/download/server/bedrock/),而不是Java版服务器.

<!-- more -->
## 为什么选择基岩版而不是 Java 版?
基岩版支持跨平台,你在床上用手机玩,他在桌前用电脑玩,我在沙发上用 XBOX 玩,而且是同服联机
* 基岩版优化远超 Java 版,帧数高
* 基岩版目前是 Mojang 的主要开发方向,更新快速,水域更新,熊猫更新等都是基岩版抢先
* 基岩版是各平台的主推版本,无需安装繁重的 Java,只不过没有盗版,想玩需要付费或使用一些技巧
* 基岩版使用 XBOX 账号 ,无论你在哪里,只要账号相同,存档就同步(1 小时前我在台式电脑上玩,现在出门用手机玩,待会儿在火车上用笔记本玩,到了酒店用 XBOX 玩)

## 下载并解压
从[Minecraft Bedrock Sever](https://minecraft.net/en-us/download/server/bedrock/)网站获取基岩服务端压缩包,解压.

## 一连串白痴问题
根据官网的引导,我使用`LD_LIBRARY_PATH=. ./bedrock_server`试图启动服务端,然后系统却无情的抛出一连串错错误
### openssl不对
`openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory`
这段错误提示摆明了告诉我openssl版本不对劲儿,`openssl version`一看,我这边装的是1.0.1.但我想要通过`apt-get install`获取1.1.0版本,可却没有这个包???
那我只能自己编译了.
#### 编译并安装openssl
我前往openssl官网下载了最新的源码包,下载并解压,准备编译与安装
```
./configure --prefix /usr/local/openssl110 #指定openssl安装目录
make #编译
make install #安装
openssl version #1.1.0
```
openssl总算安装好了,我再次启动基岩服务端
### GCC NOT FOUND
我佛了,glibc和glibc++版本又不对劲儿了.
再次前往gnu官网下载glibc和glibc++源代码并编译安装.
一番操作后,总算装好了.

我的GCC不是装在全局里的,所以我再次启动基岩服务端时加了一条so搜索目录,完整命令如下
`LD_LIBRARY_PATH=.:/path/to/gcc227 ./bedrock_server`

### Segmentation Fault
这次,我又遇到了这个与dpmpro无法在安卓9运行的同样问题的错误,这个让我在多方面绞尽脑汁也无法解决的错误.
听说这是内核错误?可我真的不懂这个啊.

### 搜寻方法
在google了一天后,我仍然无法解决问题,我重装了系统,可仍然无效.这时我想起在之前浏览基岩服务端反馈中心时看到的帖子
> Bed rock server must run on ubuntu 18.04 or later

卧槽,可我这学生机没法更换ubuntu18.04镜像啊,咋办啊?

## 从ubuntu 16.04升级到ubuntu18.04
我一直觉得linux发行版的内置升级很不可靠,但这次,我别无选择.
一番查询后,我找到了这篇文章[Upgrade Ubuntu 16.04 LTS To Ubuntu 18.04 LTS Server](https://websiteforstudents.com/upgrade-ubuntu-16-04-lts-to-ubuntu-18-04-lts-beta-server/)
按照文中内容进行操作,并且进行了重启后.我再次连接到终端,屏幕上赫然写着:
> Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-43-generic x86_64)

我居然真的将服务器更新到了Ubuntu18.04系统!那一刻,我激动地开始为自己鼓掌!

## 再次运行基岩服务器
我怀着忐忑的心情,再次执行`LD_LIBRARY_PATH=. ./bedrock_server`


> NO LOG FILE! - setting up server logging...
NO LOG FILE! - [2019-01-31 21:37:48 INFO] Starting Server
NO LOG FILE! - [2019-01-31 21:37:48 INFO] Version 1.8.1.2
[2019-01-31 21:37:48 INFO] Level Name: DreamWorld
[2019-01-31 21:37:48 INFO] Game mode: 0 Survival
[2019-01-31 21:37:48 INFO] Difficulty: 3 HARD
[2019-01-31 21:37:49 INFO] IPv4 supported, port: 19132
[2019-01-31 21:37:49 INFO] IPv6 supported, port: 19133
[2019-01-31 21:37:50 INFO] Server started.

终于,成功了.

这时的我想,如果我开的是Java版服务器,或许就不用这么折腾了,毕竟装个java直接运行就好,执行这种本地代码应用总是这么的麻烦.但立刻,我否定了这个想法,毕竟我确实在这次折腾中,学到了不少东西啊!



