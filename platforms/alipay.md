# 支付宝小程序

`@megalo/target` 的 `platform` 设置成 `alipay`，`mini-css-extract-plugin` 提取文件后缀改成支付宝小程序的 `acss`。

```javascript
const createMegaloTarget = require( '@megalo/target' )
const compiler = require( '@megalo/template-compiler' )
const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )

module.exports = {
  target: createMegaloTarget( {
    compiler,
    platform: 'alipay'
  } ),

  plugins: [
    new MiniCssExtractPlugin( {
      filename: './static/css/[name].acss',
    } )
    // ...
  ]

  // ... 其他配置
}

```