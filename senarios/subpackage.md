# 分包

支持小程序的分包机制，配置 entry 时按一般的页面进行配置，目录结构如下：

```
└─ src
  ├─ packageA
  │  └─ pages
  │     └─ a
  │       ├─ index.js
  │       └─ index.vue
  └─ pages
    └─ home
      ├─ index.js
      └─ index.vue
```

App 入口 js 按照小程序的分布方法进行配置，跳转链接按照相对路径进行配置，例如从 `home/index` 跳转的到 `packgeA/pages/a/index`，相对路径为 `../../packgeA/pages/a/index`。

```javascript
import App from './App'
import Vue from 'vue'

const app = new Vue( App )

app.$mount()

export default {
  config: {
    pages: [
      'pages/home/index',
    ],
    subPackages: [
      {
          root: 'packageA',
          pages: ['pages/a/index']
      }
    ]
  }
}
```

完整代码可以参考 [megalo-demo](https://github.com/kaola-fed/megalo-demo)。