# textarea 上使用 v-model

会在 iOS 上出现 v-model 在 textarea 上不生效的问题，原因是在 iOS 真机上，无法动态在 textarea 设置 `data-`，即：

```html
<textarea data-cid="{{cid}}" bindinput="onInput"></textarea>
```

当 `input` 事件触发时，无法从事件对象上取得 `dataCid` 的值，从而无法根据上面的信息找到对于的事件处理方法。

## 解决方法

框架必须依赖于原生标签上 `data-` 的属性找到对于的事件处理方法，在实际测试中，除了原生小程序组件意外，普通的自定组件也可以动态设置 `data-` 属性。所以可以利用自定义组件给 `textarea` 包裹的方式要绕开这个问题。

### 定义

定义自定义组件 `textarea`，组件的代码：

index.wxml

```html
<textarea
  placeholder="{{placeholder}}"
  placeholder-style="{{placeholderStyle}}"
  placeholder-class="{{placeholderClass}}"
  disabled="{{disabled}}"
  auto-focus="{{autoFocus}}"
  focus="{{focue}}"
  auto-height="{{autoHeight}}"
  fixed="{{fixed}}"
  cursor-spacing="{{cursorSpacing}}"
  cursor="{{cursor}}"
  show-confirm-bar="{{showConfirmBar}}"
  selection-start="{{selectionStart}}"
  selection-end="{{selectionEnd}}"
  adjust-position="{{adjustPosition}}"
  maxlength="{{maxlength}}"
  value="{{value}}"

  bindinput="proxy"
  bindblur="proxy"
  bindfocus="proxy"
  bindblur="proxy"
  bindlinechange="proxy"
>
</textarea>
```

index.js

```javascript
Component({
    properties: {
        placeholder: { type: String, value: '' },
        placeholderStyle: { type: String, value: '' },
        placeholderClass: { type: String, value: '' },
        disabled: { type: Boolean, value: false },
        autoFocus: { type: Boolean, value: false },
        focus: { type: Boolean, value: false },
        autoHeight: { type: Boolean, value: false },
        fixed: { type: Boolean, value: false },
        cursorSpacing: { type: Number | String, value: 0 },
        cursor: { type: Number, value: 0 },
        showConfirmBar: { type: Boolean, value: true },
        selectionStart: { type: Number, value: -1 },
        selectionEnd: { type: Number, value: -1 },
        adjustPosition: { type: Boolean, value: true },
        maxlength: { type: Number, value: 140 },
        value: { type: String, value: '' }
    },
    methods: {
        proxy(e) {
            this.triggerEvent(e.type, e.detail);
        }
    }
});
```

index.wxss

```css
textarea{
	width:inherit;
	height:inherit;
	display:inherit;
	position:inherit;
}
```

index.json

```json
{
  "component": true
}
```

### 使用

自定义组件配置：

index.js

```javascript
export default {
  config: {
    usingComponents: {
      textarea: 'path/to/components/textarea/index'
    }
  }
}
```

index.vue

```html
<textarea v-model="input"></textarea>
```

使用就和正常的使用方式一样。原生自定义组件可以通过配置 webpack copy plugin 自动拷贝到指定目录，不需要额外的编译转换（除非你使用了可能在低版本系统中不支持的语法）。

这个方法不优雅，但能绕开这个问题。如果你有更优雅的解决思路，欢迎提 [issue](https://github.com/kaola-fed/megalo/issues/)。

## 延伸

如果还碰到原生标签无法绑定事件的问题，大概率是类似的原因，即 `data-` 无法设置成功，可以尝试采用类似的解决方法。如果仍然无法解决，请提 issue。

相关 issue:

[textarea 双向绑定失败](https://github.com/kaola-fed/megalo/issues/113)
