# 应用 - App

主要生命周期的顺序为：`created` => `mounted`(`onLaunch`)。同时 `onShow`、`onHide`、`onError`、`onPageNotFound` 也会与小程序 `App` 对应的声明周期钩子绑定。

App 逻辑，`App.vue`。应用的生命周期钩子写在这里，运行时通过小程序的 `App` 方法注册应用。

```html
<template></template>
<script>
  export default {
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


## 配置

由于每个平台的配置项存在差异，在配置时以微信小程序的为准，构建时会自动将属性字段转换成平台对应的字段。

### window 转换对照

描述 | 微信 | 支付宝 | 百度
---|---|---|---
导航栏标题文字内容 | navigationBarTitleText | defaultTitle | navigationBarTitleText
导航栏背景颜色 | navigationBarBackgroundColor | titleBarColor | navigationBarBackgroundColor
导航栏标题颜色 | navigationBarTextStyle | ❌ | navigationBarTextStyle
导航栏样式 | navigationStyle | ❌ | navigationStyle
窗口的背景色 | backgroundColor | ❌ | backgroundColor
下拉 loading 的样式 | backgroundTextStyle | ❌ | backgroundTextStyle
顶部窗口的背景色 | backgroundColorTop | ❌ |
底部窗口的背景色 | backgroundColorBottom | ❌ |
是否开启当前页面的下拉刷新 | enablePullDownRefresh | pullRefresh | enablePullDownRefresh
页面上拉触底事件触发时距页面底部距离 | onReachBottomDistance | ❌ | onReachBottomDistance
屏幕旋转设置 | pageOrientation | ❌ |

### tabBar 转换对照

描述 | 微信 | 支付宝 | 百度
---|---|---|---
tab 上的文字默认颜色 | color | textColor | color
tab 上的文字选中时的颜色 | selectedColor | selectedColor |  selectedColor
tab 的背景色 | backgroundColor | backgroundColor | backgroundColor
tabbar上边框的颜色 | borderStyle | ❌  | borderStyle
tab 的列表 | list | items | list
tabBar的位置 | position | ❌  | position

每个 list 的项转换对照。

描述 | 微信 | 支付宝 | 百度
---|---|---|---
页面路径 | pagePath | pagePath | pagePath
tab 上按钮文字 | text | name |  text
图片路径 | iconPath | icon | iconPath
选中时的图片路径 | selectedIconPath | activeIcon  | selectedIconPath

### subpackages

微信和百度小程序支持 `subpackages`（或 `subPackages`，建议写成全小写），但支付宝小程序暂不支持分包，所以在支付宝小程序中，`subpackages` 中的页面会被提取出来放到 `pages` 中。 如：

```javascript
export default {
  config: {
    pages: [
      'pages/index/index',
    ],
    subPackages: [
      {
        root: 'packageA',
        pages: ['pages/a/index']
      }
    ],
  }
}
```

在支持分包的微信和百度小程序中会直接转换，但在支付宝小程序中会将分包中的页面放到 `pages` 中。（等支付宝小程序支持后会取消这一处理）

```json
{
  "pages": [
    "pages/index/index",
    "packageA/pages/a/index"
  ]
}
```

### 平台定制

如果需要针对不同的平台设置不同字段，可以根据平台在 `config._wechat`，`config._alipay`，`config._swan` 字段定制，结构与 `config` 下一致，在生成 `json` 时会将其覆盖到 `config` 上。例如：

```javascript
export default {
  config: {
    // 统一配置
    window: {
      navigationBarBackgroundColor: '#fff',
      navigationBarTitleText: 'WeChat'
    },

    // 支付宝小程序配置
    _alipay: {
      window: {
        navigationBarTitleText: 'Alipay'
      }
    },

    // 百度小程序配置
    _swan: {
      window: {
        navigationBarTitleText: 'Baidu'
      }
    }
  }
}
```

在微信小程序中没特殊配置，最终生成的 json 为：

```json
{
  "window": {
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Wechat",
  }
}
```

在支付宝小程序中 `_alipay` 下的配置项会 merge 到 config 中，最终生成的 json 为：

```json
{
  "window": {
    "titleBarColor": "#fff",
    "defaultTitle": "Alipay"
  }
}
```

在百度小程序中 `_swan` 下的配置项会 merge 到 config 中，最终生成的 json 为：

```json
{
  "window": {
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Baidu",
  }
}
```

### 平台特有字段

某些配置，如微信下的 `resizable`, 只在某个平台下支持，建议写到平台定制字段下。写到 `config` 下的话，json 生成时不会去除这些字段，但它们其他平台中不会生效。
