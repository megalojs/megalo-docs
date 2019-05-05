# API

## $mp

页面的根 VM 上在页面初始化时保存小程序的相关信息，VM 树上所有的 VM 都可以通过 this.$mp 获取

```js
{
  platform: 'wechat',   // 平台，目前有 微信:'wechat', 支付宝：'alipay',百度智能小程序：'swan'
  page: Page,           // 小程序页面实例 
  query: { id: 100 },   // onLoad 传入的页面参数对象
  options: { id: 100 }, // 与 query 是同一个对象（别名）
}
```

```js
{
  platform: 'wechat',   
  app: App,             // 小程序程序实例，@0.2.2+，注意，@0.2.1-依然定义为 page
  query: { id: 100 },   
  options: { id: 100 }, 
}
```

## 小程序 API

以微信为例，微信小程序 API 可通过全局对象 `wx` 直接调用如 `wx.api`，alipay 为 `my`，百度智能小程序为 `swan`。

如果需要做同构，那么 api 的调用需要注意平台区分。

比如我们需要使用小程序原生的网络接口 `request`，但是在不同的平台下命名有所不同。

我们可以手动利用 vue mixin 或其他方式统一 api

```js
export default {
  methods: {
    GET (api, callback) {
      let { platform } = this.$mp || {},
        request = ()=>{}
      switch(platform) {
        case 'wechat':
          request = wx && wx.request
        break;
        case 'alipay':
          request = my && my.httpRequest
        break;
        case 'swan':
          request = swan && swan.request
        break;
        default:
        break;
      }
      request && request({
        url: api,
        success: callback
      })
    }
  }
}
```

同样具有全局性质的还有全局方法 `getApp()` 等，类似的全局对象均可直接访问

## globalData

由于小程序的各个页面间都是相对独立的，官方提供了 `globalData` 用于储存全局数据

megalo 将 globalData 改造成了类似于 vue data 的构建形式，可以是对象，也可以是执行方法

```js
// object
{
  globalData: 'I am global data'
}

{
  globalData: {
    msg: 'I am global data'
  }
}

// funtion
{
  globalData: ()=>{
    return {
      mp: this.$mp
    }
  }
}
```



