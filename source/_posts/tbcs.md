---
title: 2401免费云签到上线
date: 2019-01-31 20:17:21
tags: [php,lnmp,Just4Fun]
categories: 后端
---
{% asset_img head.png%}
自从前几天在我的学生机上搭建了[Minecraft Bedrock Sever](https://minecraft.net/en-us/download/server/bedrock/)后,我就一直想再搞点什么东西,榨干我的机器性能.
<!-- more -->
## 前言
而今天逛百度贴吧的酷安评论区时,看到有人在搭建贴吧云签到,我惊了,贴吧云签到不是早就GG了吗?又活了?(2016年我搭建过一次)
再去看看贴吧云签到的[开源仓库](https://github.com/MoeNetwork/Tieba-Cloud-Sign),我靠,确实还活着,那没什么好说的了,开始搭建!

## 安装LNMP
贴吧云签作为一个基于PHP的网页应用,自然是需要WEB服务器+PHP+数据库的,对于我这种喜欢偷懒外加对这方面不熟悉的人来说,[LNMP一键安装包](https://lnmp.org/)是绝对的首选
```
screen -r lnmp #new screen
wget http://soft.vpser.net/lnmp/lnmp1.5.tar.gz -cO lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && ./install.sh lnmp #无人值守安装
```
在根据lnmp脚本的提示设置安装参数后,便可以享受lnmp一键安装包无人值守坐等完成的特性.

背了会儿单词后,LNMP安装好了.
{% asset_img lnmp_installed.png%}
## 新建虚拟主机
要是什么都放在默认目录下,那可就乱套了,好歹也得有个端口进行区分,而这样的操作可以通过vhost实现.
我的机子是ubuntu18.04,vhost配置文件位于`/usr/loca/nginx/conf/vhost/`目录,我前往这个目录,使用`vim tbcs.conf`创建并写入了贴吧云签到虚拟主机的信息,使其对本机`2402`端口的方式指向我为贴吧云签准备的目录

## 部署贴吧云签文件
在我为贴吧云签到准备的目录里,直接`wget https://github.com/MoeNetwork/Tieba-Cloud-Sign/releases/tag/V4.9`,就得到了应用程序的压缩包,`unzip`后,贴吧云签系统的文件就部署完成了.

## 为贴吧云签准备数据库
打开phpmyadmin,新建tbcs库,完事儿

## 安装贴吧云签
根据我之前的设置,访问服务器的2402端口进入到了贴吧云签的安装引导,一顿配置后,贴吧云签算是OK了.
{% asset_img tbcs_ok.png%}

## 定时任务
配置定时任务,实现贴吧云签到系统的自动任务:
`crontab -e`,并添加`* * * * * php /path/to/do.php`,每分钟执行一次贴吧云签的任务系统

## 顺便恢复我的下载站
自从为了安装[Minecraft Bedrock Sever](https://minecraft.net/en-us/download/server/bedrock/)而重置服务器以来直到今天我才恢复LNMP环境.
重新配置vhost,让对服务器4396端口的访问指向下载目录,并开启`autoindex`,大功告成,我的拓展模块可以重新上架了!
{% asset_img dl.png%}

## 欢迎使用
说了这么多,你想要使用吗?
欢迎访问[2401免费签到(已更新最新地址)](http://tb.zsh2401.top),各位大神手下留情,我只是学生,也不玩什么商业竞争,希望大家不要来黑我的服务器TAT