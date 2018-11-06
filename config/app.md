# 应用 - App

声明 `mpType` 为 `page` 作为小程序页面入口，以此绑定小程序 `App` 和 Vue 实例的声明周期。

主要生命周期的顺序为：`created` => `mounted`(`onLaunch`)。同时 `onShow`、`onHide`、`onError`、`onPageNotFound` 也会与小程序 `App` 对应的声明周期钩子绑定。

App 逻辑，`App.vue`。应用的生命周期钩子写在这里，运行时通过小程序的 `App` 方法注册应用。

```html
<template></template>
<script>
  export default {
    mpType: 'app',
    created() {
      console.log('launch');
    }
  }
</script>
```

App 入口 文件，`index.js`。应用配置写在 `config` 字段中并 export 出来，最终会生成 `app.json` 文件。

```javascript
import App from './App'
import Vue from 'vue'

const app = new Vue( App )

app.$mount()

export default {
  config: {
    pages: [
      'pages/index/index',
    ],
    window: {
      backgroundTextStyle: 'light',
      navigationBarBackgroundColor: '#fff',
      navigationBarTitleText: 'WeChat',
      navigationBarTextStyle: 'black'
    }
  }
}
```