---
title: image-webpack-loader安装慢的解决方案
date: 2019-11-30 04:10:04
tags: [webpack,typescript,性能优化,image-webpack-loader]
categories: 基础技术
---
经过搜索,我选择使用[image-webpack-loader](https://github.com/tcoopman/image-webpack-loader)对自己的web应用进行优化。然而，安装实在是太慢了，这里记录一下我的解决方案。   
<!-- more -->
image-webpack-loader实际上只是对imagemin进行了一个简单的封装，而imagemin分为于以下几个包：
* imagemin-gifsicle(gifsicle-bin)
* imagemin-jpegtran(jpegtran-bin)
* imagemin-mozjpeg(mozjpeg-bin)
* imagemin-optipng(optipng-bin)
* imagemin-pngquant(pngquant-bin)
* imagemin-svgo(svgo-bin)

可以看出，这几个都是native包。   
阅读[源代码](https://github.com/imagemin/gifsicle-bin/blob/master/lib/index.js),可以看到这几个包在安装时会下载二进制文件，但重点来了，即便我开了~~科学上网~~工具，下载速度仍然感人到极致，每次都十分钟起步，怎么办呢？   

------

我与每一个前端开发者一样，第一时间想到以换源解决这个问题。   
我使用`yarn`,`npm`换了每一个源，安装都是那么龟速。   
{% asset_image src.png %}

偶然一次机会，我在某个项目中完成了imagemin的安装，并在阅读`gifsicle-bin`的源代码后，了解到了这几个包的安装方式:   
安装时执行`lib/install.js`从github下载二进制包，由于众所周知的原因，这变得异常缓慢...   
~~令我感到疑惑的是，为什么我挂了代理依然很慢?~~

```javascript
/*
node_modules/gifsicle-bin/lib/index.js
*/
const url = `https://raw.githubusercontent.com/imagemin/gifsicle-bin/v${pkg.version}/vendor/`;

module.exports = new BinWrapper()
	.src(`${url}macos/gifsicle`, 'darwin')
	.src(`${url}linux/x86/gifsicle`, 'linux', 'x86')
	.src(`${url}linux/x64/gifsicle`, 'linux', 'x64')
	.src(`${url}freebsd/x86/gifsicle`, 'freebsd', 'x86')
	.src(`${url}freebsd/x64/gifsicle`, 'freebsd', 'x64')
	.src(`${url}win/x86/gifsicle.exe`, 'win32', 'x86')
	.src(`${url}win/x64/gifsicle.exe`, 'win32', 'x64')
	.dest(path.join(__dirname, '../vendor'))
    .use(process.platform === 'win32' ? 'gifsicle.exe' : 'gifsicle');
    
```
怎么办呢？经过一天的折腾和尝试，我发现`cnpm`可以很好的解决这个问题。   
当你使用`cnpm`安装`imagemin`时,包中的`lib/install.js`将以[淘宝镜像](http://npm.taobao.org/mirrors)代替`raw.githubusercontent.com`，这样一来，下载几乎是瞬间完成。
```javascript
const url = `https://npm.taobao.org/mirrors/gifsicle-bin/v${pkg.version}/vendor/`;
```
真的挺神奇的,cnpm好感度++   
注意：
1. 如果从yarn/npm等工具切换到cnpm，一定要将node_modules删除然后重新执行`cnpm install`，否则会导致严重的依赖错误问题。
2. 如果TypeScript报`Duplicate identifier`错误的话,在`tsconfig.exculde`中加上`"node_modules/*"`即可 
```json
//tsconfig.json
{
  ...
  "exclude": ["node_modules/*"]
  ...
}
```