#  webpack 配置

Megalo 在构建时依赖 [`@magelo/target`](https://github.com/kaola-fed/megalo-aot) 和 [`@magelo/template-compiler`](https://github.com/kaola-fed/megalo)。利用 `createMegaloTarget` 方法创建 webpack 的构建目标，通过 `platform` 和 `compiler` 配置好模版编译器和目标平台。

```javascript
const createMegaloTarget = require( '@megalo/target' )
const compiler = require( '@megalo/template-compiler' )
const VueLoaderPlugin = require( 'vue-loader/lib/plugin' )
const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )
const platform = 'wechat';      // 选择构建的平台（微信：wechat，支付宝：alipay）
const cssExt = 'wxss';          // 生产的 css 文件后缀（微信：wxss，支付宝：acss）
module.exports = {
  target: createMegaloTarget( {
    compiler,
    platform
  } ),
  // 注意：`devtool` 不能设置成 `eval` 类型的 sourcemap，如 `cheap-eval-source-map`，因为小程序中不允许执行 `eval`。
  devtool: 'source-map',
  entry: {
    // 应用入口
    'app': _.resolve( 'src/index.js' ),
    // ... cli 中封装了读取主文件 index.js 的方法，将自动生成 entry，无需再次配置
  },
  output: {
    path: _.resolve( `dist` ),
    filename: 'static/js/[name].js',
    chunkFilename: 'static/js/[id].js'
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          test: /[\\/]node_modules[\\/]|megalo[\\/]/,
          name: 'vendor',
          chunks: 'all'
        }
      }
    }
  },
  resolve: {
    extensions: ['.vue', '.js', '.json'],
    alias: {
      // 重定 vue 指向
      'vue': 'megalo', 
      '@': _.resolve('src')
    },
  },
  module: { /* 普通的 module 配置 */},
  plugins: [
    new VueLoaderPlugin(),
    new MiniCssExtractPlugin( {
      filename: `./static/css/[name].${cssExt}` // 重新定义后缀
    } )
  ]
}
```

`@megalo/target` 的职责单一，主要作用添加一些插件和 loader，用于解析和生成小程序相关的代码，因此还需要依赖其他构建工具：

- `vue-loader`：需要 `vue-loader`（^14） 加载和解析 `.vue` 文件；
- `mini-css-extract-plugin`：用来将 css 提取到 `.wxss` 文件。

> 以上配置为 Megalo 的必须配置，应尽量避免修改以免导致解析出错
