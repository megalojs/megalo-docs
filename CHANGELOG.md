## 0.2.1 (20181109)

- `A` 标签属性默认值，`<button enable>` 相当于 `<button enable="true">`
- `U` 优化数据更新性能，减少冗余属性更新
- `U` 优化 `@megalo/template-compiler` 的编译性能，配合 `@megalo/target`
- `F` 修复百度智能小程序下更新 `'a-b'` 格式属性的 bug

## 0.2.0 (20181107)

- `A` `$mp` 对象上增加 `platform` 字段，获取当前平台类型
- `A` `$mp` 对象上增加 `query` 字段，与 `options` 一致，报错 `onLoad` 传入的参数信息
- `F` 修复监听驼峰事件名时的无法找到 `handler` 的错误

## 0.2.0-0 (20181106)

- `A` 支持百度智能小程序，对应依赖更新 `@megalo/target@0.2.0-0`、`@megalo/template-compiler@0.2.0-0`
- `A` App 支持声明 globalData
- `U` 优化数据更新性能
- `F` 修复 Vuex 初始化时注册成 Page 的问题