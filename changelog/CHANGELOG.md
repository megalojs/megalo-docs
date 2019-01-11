## 0.7.3 (20190110)

- `F` 修复上个版本引入的事件绑定异常（在微信中无效，bind:xxx 对原生组件无效，改回 bindxxx）

## 0.7.2 (20190110)

- `F` 修复 @xx.capture 的生成的模版语法问题

## 0.7.1 (20190110)

- `U` 优化 scopeid 传递实现方式，解决样式延时问题
- `F` 修复 slot 中清空数组无效的问题
- `F` 修复 v-model 设置空字符串无效的问题

## 0.7.0 (20190101)

- `U` 优化生成代码大小
  - 对于原生标签，在 render 函数中不生成 staticClass、staticStyle
  - 缩短标记变量名，`_hid => h_`，`_fid => f_`，`_fk => k_`，`_cid => c_`，`__if => i_`
- `A` 支持子组件继承父组件的 scopeid
- `A` 在 compiler 的返回值增加 `children` 字段，包含子组件及其 slot 的相关信息，以给 `@megalo/target` 进行依赖分析
- `F` 修复 `v-model.lazy` 将事件绑定成 `change`，换成用 `blur` 实现
- `F` 修复列表切换时第一个元素不变的问题

## 0.6.3 (20181214)

- `F` 修复 src 为空时多生成的 `/`

## 0.6.2 (20181213)

- `F` 修复只有 static class 时的合并问题
- `F` 修复 `v-html` 在百度小程序的显示问题


## 0.6.1 (20181212)

- `F` 修复 slot 中元素属性渲染异常

## 0.6.0 (20181210)

- `U` 重写 slot 的逻辑，修复某些使用场景下出现的渲染异常
- `A` 支持 `mp:` 前缀属性，区分与 vue 语法冲突的属性名，如 `mp:key` [#71](https://github.com/kaola-fed/megalo/issues/71)
- `A` 支持小程序原生钩子 mixin [#73](https://github.com/kaola-fed/megalo/issues/73)
- `A` 支持通过 url-loader 转换后的图片资源加载 [#74](https://github.com/kaola-fed/megalo/issues/74)
- `A` 初始化组件时将组件名称更新到 AppData，用于定位
- `F` 修复 slot 中元素在某些情况下的渲染异常
- `F` 修复 attrs 编译异常
- `F` 修复计算 VMId 时触发 render 的问题
(注意：要将 `@megalo/target` 更新到 `0.5.0` 以上)

## 0.5.2 (20181203)

- `F` 修复在 Promise 中连续更新同一个值时不更新到视图层的异常

## 0.5.1 (20181202)

- `U` 支持组件上静态 class，即支持 `<comp class="comp" :class="classObject"></comp>`，[#52](https://github.com/kaola-fed/megalo/issues/52)
- `U` 优化 globalData 逻辑，在 App 中可以通过 `this.globalData` 访问，页面通过 `this.$mp.app.globalData` 访问
- `U` 优化 fallback slot 生成逻辑，在不定义的情况下也生成 fallback slot，，[#53](https://github.com/kaola-fed/megalo/issues/53)
- `F` 修复循环中组件事件问题，[#54](https://github.com/kaola-fed/megalo/issues/54)

## 0.5.0 (20181129)

- `U` 优化 `v-for` 所生成的代码
- `U` 更改 scoped style 所生成的 scoped-class 逻辑，`class="div"` -> `class="div r-123"`，只要带上 scoped 都会添加 scoped-class
- `F` 修复 `new Vue()` 不传参数时的异常
- `F` 修复嵌套 `v-for` 和 `slot` 在某些场景下的异常

## 0.4.0 (20181121)

- `A` 支持 `v-text`
- `F` 修复组件名称问题，支持注册时为 `PascalCase`、`camelCase`，模版声明为 `kebab-case` [#41](https://github.com/kaola-fed/megalo/issues/41)

## 0.3.2 (20181116)

- `F` 优化 `v-if` render 逻辑，修复 `v-if` 异常 [#33](https://github.com/kaola-fed/megalo/issues/33)
- `F` 修复数据更新异常
- `F` 修复 `this.$mp.query` 获取页面参数异常

## 0.3.1 (20181115)

- `F` 修复多组 `v-if` 显示异常的 bug

## 0.3.0 (20181113)

- `A` 生命周期修改，`onLoad` 中出发 `rootVM.$mount()`，即首次数据更新在 `onLoad` 最后阶段触发
- `A` 将 `<a href="">` 自动转换为 `<navigator url="">`
- `U` 优化 `v-if` 条件表达式的 `render` 代码，减少执行次数
- `F` 修复原生自定义组件的具名 `slot` 问题
- `F` 修复支付宝 `web-view` 无法动态绑定 `src` 的问题

## 0.2.2 (20181112)

- `U` 将 App 内的小程序实例名称改为 `app`，通过 `this.$mp.app` 获取小程序 App 实例

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