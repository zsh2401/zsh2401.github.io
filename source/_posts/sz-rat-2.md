---
title: '[sz-rat] CDN vs VENDORS?'
date: 2019-11-14 22:17:23
categories: 前端
tags: [webpack,react,sz-rat]
hidden: true
---
&ensp;&ensp;&ensp;&ensp;在现代web项目中,通常会使用大量第三方库,如React,Vue,Lodash,Bootstrap,Antd等。这些工业级的轮子为我们的开发带来便捷的同时，毫无疑问会使应用体积增大并导致加载速度变慢。   
&ensp;&ensp;&ensp;&ensp;不知你是否听过这个言论：”网站加载时间延迟1秒钟等同于：转化率丢失7%，客户满意度降低16%! “，无论这句话是否有根据，web应用加载/首屏速度都是优化的重点。
&ensp;&ensp;&ensp;&ensp;对于一个webpack打包的应用来说，可以将第三方库打包到公用vendors或使用CDN引入代码来提升加载速度。问题来了，哪一个更好呢？
<!-- more -->
### vendors
vendors有个显著的优点就是可以方便地被webpack自动配置,我们不需要去关心太多。当vendors被引用时


