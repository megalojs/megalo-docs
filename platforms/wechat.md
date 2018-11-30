# 微信小程序

`@megalo/target` 的 `platform` 设置成 `wechat`，`mini-css-extract-plugin` 提取文件后缀改成微信小程序的 `wxss`。

```javascript
const createMegaloTarget = require( '@megalo/target' )
const compiler = require( '@megalo/template-compiler' )
const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )

module.exports = {
  target: createMegaloTarget( {
    compiler,
    platform: 'wechat'
  } ),

  plugins: [
    new MiniCssExtractPlugin( {
      filename: 'static/css/[name].wxss',
    } )
    // ...
  ]

  // ... 其他配置
}

```
