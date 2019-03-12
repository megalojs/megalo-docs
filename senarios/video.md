# video 的事件

和 textarea 上的问题类似，原因在 Android 真机上，无法动态在 video 设置 `data-`，即：

```html
<video data-cid="{{cid}}" bindplay="onPlay"></video>
```

当 `play` 事件触发时，无法从事件对象上取得 `dataCid` 的值，从而无法根据上面的信息找到对于的事件处理方法。

## 解决方法

框架必须依赖于原生标签上 `data-` 的属性找到对于的事件处理方法，在实际测试中，除了原生小程序组件以外，自定组件也可以动态设置 `data-` 属性。所以可以利用自定义组件给 `video` 包裹的方式要绕开这个问题。

### 定义

定义自定义组件 `video`，组件的代码：

index.wxml

```html
<video
  src="{{src}}"
  duration="{{duration}}"
  controls="{{controls}}"
  danmuList="{{danmuList}}"
  danmuBtn="{{danmuBtn}}"
  enableDanmu="{{enableDanmu}}"
  autoplay="{{autoplay}}"
  loop="{{loop}}"
  muted="{{muted}}"
  initialTime="{{initialTime}}"
  pageGesture="{{pageGesture}}"
  direction="{{direction}}"
  showProgress="{{showProgress}}"
  showFullscreenBtn="{{showFullscreenBtn}}"
  showPlayBtn="{{showPlayBtn}}"
  showCenterPlayBtn="{{showCenterPlayBtn}}"
  enableProgressGesture="{{enableProgressGesture}}"
  objectFit="{{objectFit}}"
  poster="{{poster}}"
  showMuteBtn="{{showMuteBtn}}"
  title="{{title}}"
  playBtnPosition="{{playBtnPosition}}"
  enablePlayGesture="{{enablePlayGesture}}"
  autoPauseIfNavigate="{{autoPauseIfNavigate}}"
  autoPauseIfOpenNative="{{autoPauseIfOpenNative}}"

  bindplay="proxy"
  bindpause="proxy"
  bindended="proxy"
  bindtimeupdate="proxy"
  bindfullscreenchange="proxy"
  bindwaiting="proxy"
  binderror="proxy"
  bindprogress="proxy"
>
</video>
```

index.js

```javascript
Component({
  properties: {
    src: { type: String, value: '' },
    duration: { type: Number },
    controls: { type: Boolean, value: true },
    danmuList: { type: Array, value: [] },
    danmuBtn: { type: Boolean, value: false },
    enableDanmu: { type: Boolean, value: false },
    autoplay: { type: Boolean, value: false },
    loop: { type: Boolean, value: false },
    muted: { type: Boolean, value: false },
    initialTime: { type: Number },
    pageGesture: { type: Boolean, value: false },
    direction: { type: Number },
    showProgress: { type: Boolean, value: true },
    showFullscreenBtn: { type: Boolean, value: true },
    showPlayBtn: { type: Boolean, value: true },
    showCenterPlayBtn: { type: Boolean, value: true },
    enableProgressGesture: { type: Boolean, value: true },
    objectFit: { type: String, value: 'contain' },
    poster: { type: String, value: '' },
    showMuteBtn: { type: Boolean, value: false },
    title: { type: String, value: '' },
    playBtnPosition: { type: String, value: 'bottom' },
    enablePlayGesture: { type: Boolean, value: false },
    autoPauseIfNavigate: { type: Boolean, value: true },
    autoPauseIfOpenNative: { type: Boolean, value: true }
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
video{
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
      video: 'path/to/components/video/index'
    }
  }
}
```

index.vue

```html
<video v-model="input"></video>
```

[textarea 上使用 v-model](senarios/textarea.md)
