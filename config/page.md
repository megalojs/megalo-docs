# 页面 - Page

在小程序创建 `Page` 实例时（`onLoad` 阶段），同时会创建一个于这个实例绑定的 Vue 实例作为一个页面的根实例，并将各生命周期进行绑定。

主要生命周期的顺序为：`created`(`onLoad`) => `mounted`(`onReady`) => `beforeDestroyed`(`onUnload`)。同时 `onShow`、`onHide`、`onShareAppMessage`、`onReachBottom`、`onPullDownRefresh` 也会与小程序 `Page` 对应的声明周期钩子绑定。

在每一个 Vue 实例中，都可以通过 `this.$mp` 对象访问小程序相关的数据，例如可以通过 `this.$mp.options` 访问 `onLoad` 时传入的参数，例如 `query` 字段。

App 逻辑，`app.vue`。应用的生命周期钩子写在这里，运行时通过小程序的 `Page` 方法注册成小程序页面。

```html
<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        title: 'Megalo'
      };
    },
    create() {
      // 获取 onLoad 中的 options
      console.log(this.$mp.options);
    }
  }
</script>
```

Page 入口文件，`index.js`，页面配置写在 `config` 字段中并 export 出来，最终会生成 `app.json` 文件。

```javascript
import Index from './index.vue'
import Vue from 'vue'

const app = new Vue( Index )

app.$mount()

export default {
  config: {
    navigationBarTitleText: 'Megalo'
  }
}
```

## $mp

页面的根 VM 上在页面初始化时保存小程序的相关信息，VM 树上所有的 VM 都可以通过 `this.$mp` 获取。

```javascript
{
  platform: 'wechat',   // 平台
  page: Page,           // 小程序页面实例
  query: { id: 100 },   // onLoad 传入的页面参数对象
  options: { id: 100 }, // 与 query 是同一个对象（别名）
}
```

## 配置

由于每个平台的配置项存在差异，在配置时以微信小程序的为准，构建时会自动将属性字段转换成平台对应的字段。

描述 | 微信 | 支付宝 | 百度
---|---|---|---
导航栏背景颜色 | navigationBarBackgroundColor | titleBarColor | navigationBarBackgroundColor
导航栏标题颜色 | navigationBarTextStyle | ❌  | navigationBarTextStyle
导航栏标题文字内容 | navigationBarTitleText | defaultTitle | navigationBarTitleText
窗口的背景色 | backgroundColor | ❌  | backgroundColor
下拉 loading 的样式 | backgroundTextStyle | ❌  | backgroundTextStyle
是否全局开启下拉刷新 | enablePullDownRefresh | pullRefresh | enablePullDownRefresh
页面上拉触底事件触发时距页面底部距离 | onReachBottomDistance | ❌  | onReachBottomDistance
设置为 true 则页面整体不能上下滚动 | disableScroll | ❌  | ❌

### 平台定制

如果需要针对不同的平台设置不同字段，可以根据平台在 `config._wechat`，`config._alipay`，`config._swan` 字段定制，结构与 `config` 下一致，在生成 `json` 时会将其覆盖到 `config` 上。例如：

```javascript
export default {
  config: {
    navigationBarTitleText: 'Index',

    // 微信小程序配置
    _wechat: {
      navigationBarBackgroundColor: '#44B549',
      navigationBarTextStyle: 'white'
    },

    // 支付宝小程序配置
    _alipay: {
      navigationBarBackgroundColor: '#4D7AF4',
    },

    // 百度小程序配置
    _swan: {
      navigationBarBackgroundColor: '#38f',
      navigationBarTextStyle: 'white'
    }
  }
}
```

微信小程序生成 json 为：

```json
{
  "navigationBarTitleText": "Index",
  "navigationBarBackgroundColor": "#44B549",
  "navigationBarTextStyle": "white"
}
```

支付宝小程序生成 json 为：

```json
{
  "defaultTitle": "Index",
  "titleBarColor": "#4D7AF4"
}
```

百度小程序生成 json 为：

```json
{
  "navigationBarTitleText": "Index",
  "navigationBarBackgroundColor": "#38f"
}
```

### 平台特有字段

某些配置，如微信下的 `pageOrientation`, 只在某个平台下支持，建议写到平台定制字段下。写到 `config` 下的话，json 生成时不会去除这些字段，但它们其他平台中不会生效。
