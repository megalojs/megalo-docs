# 页面 - Page

声明 `mpType` 为 `page` 作为小程序页面入口，在小程序创建 `Page` 实例时（`onLoad` 阶段），同时会创建一个于这个实例绑定的 Vue 实例作为一个页面的根实例，并将各生命周期进行绑定。

主要生命周期的顺序为：`created`(`onLoad`) => `mounted`(`onReady`) => `beforeDestroyed`(`onUnload`)。同时 `onShow`、`onHide`、`onShareAppMessage`、`onReachBottom`、`onPullDownRefresh` 也会与小程序 `Page` 对应的声明周期钩子绑定。

在每一个 Vue 实例中，都可以通过 `this.$mp` 方法小程序相关的数据，例如可以通过 `this.$mp.options` 访问 `onLoad` 时传入的参数，例如 `query` 字段。

App 逻辑，`app.vue`。应用的生命周期钩子写在这里，运行时通过小程序的 `Page` 方法注册成小程序页面。

```html
<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>
<script>
  export default {
    mpType: 'page',
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