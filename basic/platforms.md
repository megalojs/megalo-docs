## 说明

由于在跨端开发中，必不可少的会遇到不同端需要有不同实现的情况。参考滴滴chameleon中的多态，megalo中实现了类似的跨平台兼容方案。需要使用时，请**保证`@megalo/target`的版本号大于或等于0.7.2**。

## js的跨平台兼容

megalo中下面两种形式的引用会被特殊处理：

- `[path-to-name]/[name]/index.mpjs`
- `[path-to-name]/[name]`

遇到这两种形式的路径时，megalo会到`[path-to-name]/[name]`目录下查找，并根据当前平台，返回不同的文件。

具体的逻辑为：

- 先查找是否有符合当前平台的文件`index.[platform].js`，如有则返回对应文件
- 再查找`index.js`，如有则返回
- 如以上两种情况都不符合，则抛出错误

下面是一个包含跨平台逻辑的公用方法：

```
- utils
    - api1
        - index.js
        - index.wechat.js
        - index.swan.js
        - index.alipay.js
        - common.js // 可能有多平台公用的代码，但文件名可随意
```

使用`api1`时，通过`import api from utils/api1/index.mpjs`（或`import api from utils/api1`）来引用，按照上面的目录，如果在微信平台下进行编译，则最终会获取到`index.wechat.js`中的内容。

目前megalo支持的三种平台对应的文件名如下：

- 微信小程序：`index.wechat.js`
- 支付宝小程序：`index.alipay.js`
- 百度智能小程序：`index.swan.js`

可以在对应平台的文件中使用该平台的api，实现该平台中的逻辑。   
各平台共通的逻辑仍然可以再提取出来，放到一个`common.js`中。  

**建议：** 仅在大量的跨平台逻辑需要处理时使用该方式，否则使用条件语句处理即可

**说明：** 使用`[path-to-name]/[name]/index.mpjs`方式引用暂不支持ts文件，但是使用`[path-to-name]/[name]`形式引用时，可以通过覆盖webpack配置中的resolve.extensions支持ts文件

## vue单文件组件的跨平台兼容

1. template部分的跨平台兼容

以template标签为标记，通过platform属性指定平台，非当前平台的模板在编译时会被移除。没有指定platform属性的template标签被认为是所有平台都可存在的。

```
<template platform="wechat">
    <!-- 微信小程序部分模板 -->
</template>
<template platform="alipay">
    <!-- 支付宝小程序部分模板 -->
</template>
<template platform="swan">
    <!-- 百度智能小程序部分模板 -->
</template>
```

**注意：**

- platform属性值仅接收字符串，不要使用数据绑定
- 不要在template标签上使用指令，因为会被直接移除
- 不同于js的跨平台兼容，模板语法仍然参照megalo支持的vue的模板语法，不要在特定平台的模板中使用对应平台的语法

以下面的模板为例:

```
<template>
    <div>
    1
        <template platform="wechat">
        2
        </template>
        <template platform="swan">
        3
        </template>
    4
        <template platform="wechat">
        5
        </template>
    </div>
</template>
```
所有的模板都包裹在template中，并且请仍然保证根节点只有一个元素。  
在微信中进行编译后，上面的内容最终将变成：

```
<template name="defaultName">
    <view class="_div">
    1
        
        2
         
    4
        
        5
    </view>
</template>
```


2. style部分的跨平台兼容

类似于template部分的跨平台，style部分也采用platform进行标记。没有指定platform的style标签在所有平台均存在，指定了platform的标签在非当前平台编译时对应的css会从最后合并的css中移除。

```
<!-- 微信小程序的样式 -->
<style platform="wechat"></style>
<!-- 支付宝小程序的样式 -->
<style platform="alipay"></style>
<!-- 百度智能小程序的样式 -->
<style platform="swan"></style>
```

**注意：** 
- style标签支持的scoped/lang/src等属性，这里同样可以使用，相比正常的style标签只是多了一个platform属性，没有其他任何区别
- 不同于template，没有使用style标签包裹style标签的使用方法，多个样式使用多个style标签即可

3. js部分的跨平台兼容

请使用上一部分中js的跨平台兼容方式处理vue单文件组件中需要做跨平台处理的js部分内容。