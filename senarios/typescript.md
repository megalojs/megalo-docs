# 使用 typescript

Typescript工程 入口需要增加下面配置（TODO：待完善）

```javascript
import { Component } from 'vue-property-decorator';

declare module "megalo/types/vue" {
  interface Vue {
    $mp: any
  }
}

Component.registerHooks([
  'onShow',
  'onHide',
  'onShareAppMessage',
  'onReachBottom',
  'onPullDownRefresh'
])
```