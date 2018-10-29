# 支持特性

## 基本语法

支持 Vue 的基本模版语法，包括 v-for、v-if 等。

```html
<!-- v-if & v-for -->
<div v-for="(item, i) in list">
  <div v-if="isEven(i)">{{ i }} - {{ item }}</div>
</div>

<!-- style & class -->
<div :class="classObject"></div>
<div :class="{ active: true }"></div>
<div :class="[activeClass, errorClass]"></div>
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
<div :style="styleObject"></div>
<div :style="[baseStyles, overridingStyles]"></div>
```

注：class 暂时不能用在组件上

## 复杂表达式、方法调用

支持在模版中写复杂的表达式、和调用 methods 上的方法。

```html
<div>
  <div>{{ message.toUpperCase() }}</div>
  <div>{{ toUpperCase(message) }}</div>
</div>
```

## filter

支持 filter

```html
<div>
  <div>{{ message | toUpperCase }}</div>
  <div>{{ date | dateFormatter('yyyy-MM-dd') }}</div>
</div>
```

## slot

支持基本 slot 功能，包括具名 slot。

```html
<div>
  <Container>
    <Card>
      <div slot="head"> {{ title }} </div>
      <div> I'm body </div>
      <div slot="foot"> I'm footer </div>
    </Card>
  </Container>
<div>
```

注：暂不支持将 slot 一层层传递下去，例如

CompA template:

```html
<div>
  <CompB>
    <slot></slot>
  </CompB>
</div>
```

CompB template:

```html
<div>
  <slot></slot>
</div>
```

page template:

```html
<div>
  <CompA>
    page title: {{ title }}
  </CompA>
</div>
```

目前 CompA 无法将从 page 接收到的 slot 片段传递给它的子组件 CompB。

## slot-scope

支持 slot-scope。

page template:

```html
<div>
  <CompA>
    <span slot-scope="scopeProps">{{ scopeProps.item }}</span>
  </CompA>
</div>
```

CompA template:

```html
<div v-for="(item, i) in list">
  {{ i }} - <slot :item="item"></slot>
</div>
```

## v-html

`v-html` 需要添加插件 `@megalo/vhtml-plugin`，并引入模版解析库 [octoparse](https://github.com/kaola-fed/octoparse)

webpack 配置，指定解析库的路径、和名称。

```javascript
{
  // ...
  target: createMegaloTarget( {
    // ...
    htmlParse: {
      templateName: 'octoParse',
      src: _.resolve('./node_modules/octoparse/lib/platform/wechat')
    }
  } )
}
```

页面入口安装插件

```javascript
import Vue from 'vue'
import VHtmlPlugin from '@megalo/vhtml-plugin'

Vue.use(VHtmlPlugin)
```

模版中使用

```html
<div v-html="'<h1>megalo</h1>'"></div>
```

## 事件

除了支持事件绑定以外，还支持部分修饰符

```html
<button @click="onClick"></button>
```

支持修饰符：

- stop，用小程序 catch 绑定事件实现，例如 `@tap.stop` => `catchtap`
- capture，用小程序的 capture 绑定事件实现，例如 `@tap.capture` => `capture-bind`（支付宝小程序不支持）
- self（实验），目前利用特定的 data-set 标记元素实现
- once，模拟 removeListener 实现