# 代码结构

## 基本原理

megalo 在执行编译命令后会将工程文件编译成[小程序的代码结构](https://developers.weixin.qq.com/miniprogram/dev/quickstart/basic/file.html)，此处以微信小程序为例，输出 `JSON 配置`、`WXML 模板`、`WXSS 样式文件`和 `JS 逻辑交互文件`。在 JS 执行时创建 vue runtime，通过 VNode 管理工程结构，调用微信小程序的 `setData` 将数据交由 `WXML 模板` 展示。

## megalo 代码结构

我们在 cli 构建的项目中看到，Megalo 的工程结构同传统的 vue 工程基本相同，只不过暂不支持 router 所以每页都是一个 vue 的实例。

每个单页由三种文件组成：

1. `.js` 为后缀的 entry 文件
2. `.vue` 为后缀的业务源码文件
3. 其他文件，`/components`、`/static` 这类文件，也就是次要文件

<img src="../static/imgs/file-1.jpg" width="260" style="box-shadow:0 0 10px #666"> 

### 优化结构

`@megalo/target` 在版本 `0.4.10` 后可将 `.js` 和 `.vue` 合并，根据实际情况再进行拆分

新结构如下

```catalogue
├── src                      
│   ├── components          
│   ├── pages                     
│   │   ├── my.vue             
│   │   └── hello.vue                       
│   ├── static         
│   ├── app.js                   
│   ├── App.vue                  
│   └── .*           
└── .*   
``` 

接下来我们分别看看这几种文件的作用。

## entry 文件

entry 文件有两种，一种是 `/src` 下的 `app.js` 工程主文件和 `/src/pages` 下的 `hello/hello.js` page entry 文件，两种文件在功能和结构上都是很相似的。

以 `app.js` 工程主文件为例：

```js
import App from './App'
import Vue from 'vue'
import VHtmlPlugin from '@megalo/vhtml-plugin'
Vue.use(VHtmlPlugin)
const app = new Vue(App)
app.$mount()

export default {
  config: {
    // 页面前带有 ^ 符号的，会被编译成首页，其他页面可以选填，我们会自动把 webpack entry 里面的入口页面加进去
    pages: [
      'pages/hello/hello'
    ],
    window: {
      backgroundTextStyle: 'light',
      navigationBarBackgroundColor: '#fff',
      navigationBarTitleText: 'test',
      navigationBarTextStyle: 'black'
    }
  }
}
```

`app.js` 在执行编译命令后会由 megalo 编译生成两个文件 `JSON 配置文件`和 `JS 逻辑交互文件`，

```json
{
  "pages": [
    "pages/hello/hello"
  ],
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "test",
    "navigationBarTextStyle": "black"
  }
}
```

```js
require('./static/js/vendor.js')
require('./static/js/app.js')
```

json 文件就不介绍了，功能和结构请参考[微信小程序官方文档|代码结构](https://developers.weixin.qq.com/miniprogram/dev/quickstart/basic/file.html#json-%E9%85%8D%E7%BD%AE)，这里稍微解释一下 js 逻辑交互文件，虽然看上去变成了两个引用，但实际上依然是小程序的执行逻辑，注册程序 `App(Object)` 和注册页面 `Page(Object)`

## vue 文件

现在有了 `JSON 配置`和 `JS 逻辑交互文件`，还差的 `WXML 模板`和 `WXSS 样式文件` 就是由 vue 文件编译输出的。

```html
<import src="../../components/hello$e027dc2e.wxml" />
<template is="hello$e027dc2e" data="{{ ...$root['0'], $root }}"/>
```

```css
@import "../../htmlparse/index.wxss";
@import "../.././static/css/pages/hello/hello.wxss";
```

关于小程序的 `WXML 模板`和 `WXSS 样式文件`解释，详见[微信小程序官方文档|代码结构](https://developers.weixin.qq.com/miniprogram/dev/quickstart/basic/file.html#wxml-%E6%A8%A1%E6%9D%BF)

## js 文件和 vue 文件合并写法

```vue
<template>
  <div class="app">
    <!-- ... -->
  </div>
</template>

<config>
{
  "navigationBarBackgroundColor": "#ffffff",
  "navigationBarTextStyle": "black",
  "navigationBarTitleText": "微信接口功能演示",
  "backgroundColor": "#eeeeee",
  "backgroundTextStyle": "light"
}
</config>

<script>
export default {
  // ...
}
</script>

<style scoped>
  // ...
</style>

```

> 当需要对单页 Vue 对象赋能等操作时再采用独立写法

## static 静态文件

在 `static` 静态文件夹里存放的文件会在打包时 `copy` 到打包文件的 `static` 文件夹下。

可通过绝对路径 `\static\imgs\logo.png` 访问

注意:如果你正在使用最新版本的 `@megalo/cli` 创建项目,那么上述规则将不生效,我们不推荐粗暴的将静态资源直接拷贝,因为你在页面中通过`img` 标签的 `src` 属性引用的图片、`require(图片相对路径)` 函数引用图片，都会被 `file-loader` 处理并拷贝到dist-wechat目录，相关webpack配置看[这里](https://github.com/bigmeow/megalo-cli/blob/08357c839ed0aed4ce50fba3b3eb21778b37b584/packages/%40megalo/cli-plugin-mp/index.js#L136),如果你直接拷贝，就会重复多出一份静态资源

但是有些情况下，我们的确需要图片拷贝，比如小程序首页底部的tab图标的存放位置，它不属于vue框架管理，这时推荐将此资源放置到 `src/native` 目录下， 此目录主要存放不属于框架管理的代码(小程序原生组件)和静态资源

如果 "我不听我不听,就是需要拷贝" , 你可以在 `megalo.config.js` 里加入下面的代码:
```js
{
  chainWebpack: chainConfig => {
    // 你可以在这里通过 https://github.com/neutrinojs/webpack-chain 来精细的修改webpack配置
    const path = require('path')
    function resolve (_path) {
      return path.resolve(process.cwd(), _path)
    }
    chainConfig.plugin('copy-webpack-plugin').tap(oldArgs => {
      oldArgs[0].push({
        context: resolve('src/static'),
        from: `**/*`,
        to: resolve(`dist-${process.env.PLATFORM}/static`)
      })
      return oldArgs
    })
  },
}
```
为了验证你的修改是否有效, 你可以在 `package.json` 文件 `scripts` 中加一条命令:
```
 "debug": "megalo-cli-service inspect plugins"
```
然后执行
```bash
npm run debug
```