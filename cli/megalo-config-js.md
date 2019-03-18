# megalo.config.js

`megalo.config.js` 是一个可选的配置文件，如果项目的 (和 `package.json` 同级的) 根目录中存在这个文件，那么它会被 `@megalo/cli-service` 自动加载。

这个文件应该导出一个包含了选项的对象：

``` js
// megalo.config.js
module.exports = {
  // 选项...
}
```

## lintOnSave

- Type: `boolean` | `'error'`
- Default: `true`

  是否在开发环境下通过 [eslint-loader](https://github.com/webpack-contrib/eslint-loader) 在每次保存时 lint 代码。这个值会在 [`@megalo/eslint-config-standard`](https://github.com/megalojs/eslint-config-standard) 被安装之后生效。

  目前eslint默认使用 `standard` 编码规范，后续将会支持更多规范。

  设置为 `true` 时，`eslint-loader` 会将 lint 错误输出为编译警告。默认情况下，警告仅仅会被输出到命令行，且不会使得编译失败。

  如果你希望让 lint 错误在开发时直接显示在客户端控制台中，你可以使用 `lintOnSave: 'error'`。这会强制 `eslint-loader` 将 lint 错误输出为编译错误，同时也意味着 lint 错误将会导致编译失败。


  当 `lintOnSave` 是一个 truthy 的值时，`eslint-loader` 在开发和生产构建下都会被启用。如果你想要在生产构建时禁用 `eslint-loader`，你可以用如下配置：

  ``` js
  // megalo.config.js
  module.exports = {
    lintOnSave: process.env.NODE_ENV !== 'production'
  }
  ```


## productionSourceMap

- Type: `boolean`
- Default: `false`

  如果你需要生产环境的 source map，可以将其设置为 `true` 以方便生产环境代码的调试，注意开启后减慢生产环境构建速度。


  ## configureWebpack

- Type: `Object | Function`

  如果这个值是一个对象，则会通过 [webpack-merge](https://github.com/survivejs/webpack-merge) 合并到最终的配置中。

  如果这个值是一个函数，则会接收被解析的配置作为参数。该函数及可以修改配置并不返回任何东西，也可以返回一个被克隆或合并过的配置版本。

  更多细节可查阅：[配合 webpack > 简单的配置方式](../guide/webpack.md#简单的配置方式)


## chainWebpack

- Type: `Function`

  是一个函数，会接收一个基于 [webpack-chain](https://github.com/mozilla-neutrino/webpack-chain) 的 `ChainableConfig` 实例。允许对内部的 webpack 配置进行更细粒度的修改。


## css.loaderOptions

- Type: `Object`
- Default: `{}`

  向 CSS 相关的 loader 传递选项。例如：

``` js
  module.exports = {
    css: {
      loaderOptions: {
        css: {
          // 这里的选项会传递给 css-loader
        },
        px2rpx: {
          // 这里的选项会传递给 px2rpx-loader
        }
      }
    }
  }
```

支持的 loader 有：

- [css-loader](https://github.com/webpack-contrib/css-loader)
- [sass-loader](https://github.com/webpack-contrib/sass-loader)
- [less-loader](https://github.com/webpack-contrib/less-loader)
- [stylus-loader](https://github.com/shama/stylus-loader)
- [px2rpx-loader](https://github.com/megalojs/megalo-px2rpx-loader)

例如,默认项目会把 `px` 单位转换成 `rpx` 单位, 转换比例为 `1px` 等于 `2rpx`, 默认的配置为:
``` js
// megalo.config.js
module.exports = {
    css: {
        loaderOptions: {
            px2rpx: {
                rpxUnit: 0.5
            }
        }
    }
}
```

如果你想把转换比例换成 1:1 ， 可以这样配置：

``` js
// megalo.config.js
module.exports = {
    css: {
        loaderOptions: {
            px2rpx: {
                rpxUnit: 1
            }
        }
    }
}
```

如果你压根不想使用 `px2rpx` , 则直接传一个 `false` 值即可:

``` js
// megalo.config.js
module.exports = {
    css: {
        loaderOptions: {
            px2rpx: false
        }
    }
}
```

::: tip 提示
 相比于使用 `chainWebpack` 手动指定 loader 更推荐上面这样做，因为这些选项需要应用在使用了相应 loader 的多个地方。
:::

## nativeDir

- Type: `String`
- Default: `src/native`

原生小程序组件存放目录

如果你有多个平台的原生组件，你应当在此目录下再新建几个子文件夹，我们约定，子文件夹名和平台的名字一致:

微信小程序组件则命名为 'wechat'，支付宝为'alipay', 百度为 'swan'

如果只有一个平台，则无需再新建子文件夹



