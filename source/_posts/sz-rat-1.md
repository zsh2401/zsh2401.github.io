---
title: 【SZ-RAT】使用Dynamic Imports时引出的思考
date: 2019-11-08 14:50:55
tags:
---
为了让sz-rat支持更多特性,我决定通过Dynamic Imports来对代码进行分割并实现真正的按需加载.  
但令我感到困惑的是,即便代码与文章一字不差,DI的目标代码块仍然被打包到了主要代码块中.
如何解决这个问题呢? 
<!-- more   -->
文中简称
DI : Dynamic Imports
CS : Code Spliting
# 问题描述
在阅读[Dynamic Imports | webpack guides](https://webpack.js.org/guides/code-splitting/#dynamic-imports)后,我构建了[LoadableComponent](https://github.com/zsh2401/sz-rat/tree/master/src/view/components/LoadableComponent)封装动态导入逻辑,并添加了事件处理和不同状态下的视图机制.该组件参考了react-loadable的设计.
基础的测试完成后,我通过下图的代码对其进行使用,然后通过`yarn build`指令进行打包.
{% asset_img importx.png%}   
{% asset_img build1.png%}   
令我惊讶的是,webpack所称的"无需配置即可分离"并没有实现,大小超过400KB的`DyncImportTestPage`被打包到了app中.
# 尝试
### LoadableComponent的问题?
我首先想到的原因就是LoadableComponent出问题了了,于是我使用react-loadable来进行对比.
```javascript
function Loading(){
    return <div></div>
}
const Fuck = Loadable({
    loader :()=> import("../DyncImportTestPage"),
    loading:Loading
});
<Fuck/>
```
什么?结果仍然一样!`DyncImportTestPage`并未被分离.而经过一些模拟测试后,LoadableComponent确实是没有问题的.
### webpack配置有误?
webpack声称无需额外配置即可通过DI实现CS,但我此时怀疑可能是某些配置上出错,导致DI的实现出了问题.
我对整个`optimization.splitChunks`相关配置修改了N次,将分离条件,vendors等等改了一次又一次,仍然没有解决问题.
```typescript
optimization: {
        ...
		splitChunks: {
			cacheGroups: {
				vendors: {
					name:"vendors",
					priority: -10,
					chunks:'initial',
					test: /[\\/]node_modules[\\/]/
				},
				default: {
					minChunks: 2,
					priority: -20,
					reuseExistingChunk: true
				}
			},
			name: true,
			chunks: 'all',
			minChunks: 1,
			minSize: 300000,
			maxSize:0
		}
}
```
# 什么情况呢?
webpack配置没错,我写的组件也没错,那到底是哪里出了问题呢?查阅了无数文档,看了无数"最佳实践"的仓库,翻了不知多少个StackOverFlow上的问题...仍然没有找到问题根源...

灵光一闪,我想到会不会是webpack没有识别到import?是的,有可能!我突然想起来,在项目的tsconfig配置中,模块引用使用的是commonjs机制,而import是es6的玩意儿!
当ts代码被交给ts-loader和tsc进行编译时,编译结果中所有的import都变成了require,webpack找不到任何import,也就没有DI的处理了.
找到了问题根源,解决起来也就简单了,我们在`tsconfig.json`中将模块引用改成es6以上就行了↓
```json
{
  "compilerOptions": {
    ...
    "#old_module":"commonjs",
    "module": "esnext",
    ...
  }
}
```
再次编译
{% asset_img buildsuccess.png%}  
成功了!显然,`DyncImportTestPage`的代码块已经被分离!

# 思考
经过许多的尝试,最终解决问题的方案却非常简单,仅仅只是修改了typescript编译参数而已.
这个由JS模块标准与ECMAScript标准之间的碰撞所引出的问题让我更加深刻地理解了webpack,ECMAScript,JavaScript,TypeScript之间错综复杂的关系.
### 另外
项目中的webpack配置文件也是ts,但配置文件ts不支持非commonjs语法,所以为配置文件ts单独指定了一个tsconfig.json.如果你也遇到类似问题,请参考[使用不同语言进行配置webpack](https://webpack.docschina.org/configuration/configuration-languages/)


