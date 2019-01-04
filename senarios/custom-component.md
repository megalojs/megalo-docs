# 使用原生自定义组件

支持和小程序的原生自定义组件混用，使用方式和原生的使用方式没有区别。在引用时在页面 js 入口配置 `config.usingComponents`。

```javascript
export default {
  config: {
    usingComponents: {
      comp: 'path/to/components/comp/index'
    }
  }
}
```

另外还需要在 webpack 中配置 copy plugin 将原生组件的源码文件拷贝到指定位置，如果使用了特殊语法还需要进行编译转换。

## 特殊语法 mp:[attr]

原生自定义组件会给自己设置一些 `props`，属性名没有做特殊限制，因此可能会和 Vue 的语法发生冲突。例如 `key`，在非循环中给组件设置 `key` 会在编译时报错。为了规避这类冲突，设定一个特殊语法 `mp:[attr]`。

```html
<!-- 静态值 -->
<comp mp:key="1"></comp>

<!-- 动态绑定 -->
<comp :mp:key="key"></comp>
```

在构建过程中 `mp:key="1"` 会转换成 `key="1"`，`:mp:key="key"` 会转换成 `key="{{h[0]['key']}}"`。

## 延伸

相关 issue

[[feature] 模版增加 mp: 作为区分与 vue 语法冲突的字段](https://github.com/kaola-fed/megalo/issues/71)