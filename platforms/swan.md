# 百度智能小程序

`@megalo/target` 的 `platform` 设置成 `swan`，`mini-css-extract-plugin` 提取文件后缀改成百度智能小程序的 `css`。

```javascript
const createMegaloTarget = require( '@megalo/target' )
const compiler = require( '@megalo/template-compiler' )
const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )

module.exports = {
  target: createMegaloTarget( {
    compiler,
    platform: 'swan'
  } ),

  plugins: [
    new MiniCssExtractPlugin( {
      filename: 'static/css/[name].csss',
    } )
    // ...
  ]

  // ... 其他配置
}

```
