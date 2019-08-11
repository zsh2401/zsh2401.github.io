---
title: 多站点重新部署
date: 2019-08-11 13:56:38
tags: [博客维护]
categories: 基础技术
---
我在Github Pages上部署了多个静态站点,但由于不可抗力,访问速度感人。
现在，有必要着手于解决这个问题。
<!-- more -->
目前,我部署在GH Pages上的站点分别是:
* https://www.y4a.fun 慕秋
* https://www.zsh2401.top 2401的晚秋咖啡
* https://www.atmb.top 秋之盒官网

之前就有许多人反馈打不开站点,但由于Github Pages和Cloud Falre在大陆访问本就不稳定,而当时的我也别无选择,所以没有考虑重新部署。

到了2019年8月10日,
{% asset_img reason.png%}
**连她都打不开我的博客了**,我的博客是否已经失去了仅剩的意义?

延迟这么感人,更别说对其进行http访问了
{% asset_img zping.png%}

**立刻,马上!更换部署位置**

# 确定部署目标
根据资料搜寻,我发现国内托管平台码云与Coding均提供了pages服务.
我需要做出选择。
### 对比
#### 相同点
两家服务的服务器都位于香港,访问速度可以保证,同时无需备案。
#### Gitee Pages
想要自定义域名以及开启https均需要购买Gitee Pages Pro服务,并且是每个站点都要单独购买一份,价格是99RMB/Y，这实在太令人肉疼了。
#### Coding Pages
该死的注册登录流程花了我30分钟,但https和自定义域名均免费。

**毫无疑问，就决定是Coding了**
# 开始部署
## 添加公钥
想要将编译后的文件部署到仓库,就必须有权限。
## 部署zsh2401.top
### 创建私有仓库并部署
在Coding这边建立仓库,开启pages服务,并将编译后代码进行初次的部署.
### 设置自定义域名
在Coding Pages这边将项目域名绑定到zsh2401.top
{% asset_img bind.png%}
### 修改DNS与解析记录
前往阿里云,将dns服务器恢复到阿里云解析,不再使用CloudFlare,然后再前往阿里云解析,将@与www记录CNAME到Coding Pages指定的地址
{% asset_img dns.png%}
### 解析生效
在几十分钟的等待后,解析完全生效
{% asset_img ping.png%}
### 开启https
没有https的站点是没有灵魂的,我们通过Coding Pages一键申请https证书并强制启用
{% asset_img requestforhttps.png%}
That's good
{% asset_img good.png%}

其它站点不再赘述,操作大同小异

下篇博文见



<!-- more -->
