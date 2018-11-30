## 0.4.5 (20181130)

- `F` 修复 mpType: 'app' 缺失的问题

## 0.4.4 (20181130)

- `A` 允许代码中不主动声明 `mpType`

## 0.4.3

- `F` 修复和 `CopyWebpackPlugin` 混用时，拷贝输出路径多一层分包前缀的问题

## 0.4.2 (20181129)

- `F` Cannot read property 'HTMLPARSE_STYLE_OUTPUT_PATH' of undefined

## 0.4.1 (20181129)

- `A` 兼容 mpregular 编译，支持混合工程编译
- `A` 兼容 babel 7 的 `babel.config.js` 配置
- `A` 重新实现分包逻辑，不需要用户传入 subPackages，同时修复了若干问题

## 0.4.0 (20181129)

- `A` 优化分包，大幅度减小主包体积

## 0.3.0 (20181116)

- `A` 以微信小程序配置项为标准，构建时转换成支付宝、百度小程序对应字段。
- `A` 设置 `_wechat`、`_alipay`、`_swan` 等平台定制属性配置

## 0.2.2 (20181113)

- `F` 修复模版编译时的缓存问题
