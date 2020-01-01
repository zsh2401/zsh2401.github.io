---
title: php mysqli找不到的某种错误解决方案
date: 2019-12-08 17:36:03
tags: [php,后端,linux,ubuntu,mysql,数据库]
categories: 后端
---
[2401免费签到](http://tb.zsh2401.top)的自动签到任务（使用`crontab`定时执行`php /path/to/do.php`）出现了问题，日志提示`2000`错误，层层排查发现问题在于php命令行无法找到`mysqli`这个类。这个异常出现得莫名其妙，因为此前一直都正常工作。
<!-- more -->
经过几个小时的检查与测试，发现我的服务器上似乎安装了两个版本php?   

下图分别是`php -v` (`/usr/bin/php`)以及`/usr/local/php/bin/php -v`的执行结果，可以看出并不是一个版本。   
{%asset_img cli.png%}   

既然如此，我尝试使用`/usr/local/php/bin/php /path/to/do.php`命令，终于,贴吧云签到的任务被成功执行了√。   
知其然不知其所以然，这个解决方法并没有解决`/usr/bin/php`在命令行模式下无法找到`mysqli`的问题，也没有找到这个BUG突然出现的原因。
