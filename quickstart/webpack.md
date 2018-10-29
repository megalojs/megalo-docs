#  webpack 配置

Megalo 在构建时依赖 [`@magelo/target`](https://github.com/kaola-fed/megalo-aot) 和 [`@magelo/template-compiler`](https://github.com/kaola-fed/megalo)。利用 `createMegaloTarget` 方法创建 webpack 的构建目标，通过 `platform` 和 `compiler` 配置好模版编译器和目标平台。

```javascript
const createMegaloTarget = require( '@megalo/target' )
const compiler = require( '@megalo/template-compiler' )

const MiniCssExtractPlugin = require( 'mini-css-extract-plugin' )
const VueLoaderPlugin = require( 'vue-loader/lib/plugin' )

// 选择构建的平台（微信：wechat，支付宝：alipay）
const platform = 'wechat';

// 生产的 css 文件后缀（微信：wxss，支付宝：acss）
const cssExt = 'wxss';

module.exports = {
  target: createMegaloTarget( {
    compiler,
    platform
  } ),

  entry: {
    // 应用入口
    'app': _.resolve( 'src/index.js' ),
    // page 页面入口
    'pages/index/index': _.resolve( 'src/pages/index/index.js' ),
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
      'vue': 'megalo',
      '@': _.resolve('src')
    },
  },

  module: {
    rules: [
      // ... other rules
      {
        test: /\.vue$/,
        use: [
          {
            loader: 'vue-loader'
          }
        ]
      },

      {
        test: /\.js$/,
        use: 'babel-loader'
      },

      // 利用 MiniCssEtractPlugin 提取 css 到 wxss/acss
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader'
        ]
      },

      // 选择你需要的预处理器
      {
        test: /\.less$/,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          'less-loader',
        ]
      }
    ]
  },

  plugins: [
    new VueLoaderPlugin(),
    new MiniCssExtractPlugin( {
      filename: `./static/css/[name].${cssExt}`
    } )
  ]
}
```

`@megalo/target` 的职责单一，主要作用添加一些插件和 loader，用于解析和生成小程序相关的代码，因此还需要依赖其他构建工具：

- `vue-loader`：需要 `vue-loader`（^14） 加载和解析 `.vue` 文件；
- `mini-css-extract-plugin`：用来将 css 提取到 `.wxss` 文件。