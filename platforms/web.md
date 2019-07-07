megalo目前支持web编译，但由于配套设施尚不完善，功能支持还比较局限。同时由于未经过完整的测试，也可能存在一些问题。需要使用时，请仔细参考下面的说明。遇到问题请通过issue反馈给我们，同时也欢迎pr。

demo可以参考[这里](https://github.com/mosquito1803/megalo-web-bundle-demo/blob/master)

## 依赖安装

1. 安装@megalo/target@0.7.4-0版本（或更高版本），这个保证能正常编译
2. 安装@megalo/cli-plugin-web@1.0.0-alpha.3，这个用来注入web编译的配置
3. 安装vue-template-compiler@2.5.17，这个用于模板编译
4. 安装vue@2.5.17，这个提供web bundle运行时
5. 安装vue-router，这个提供路由功能

## web编译模板

web编译需要在src目录下新增了一个web目录用于存放模板

```
- src
    - pages
    - web
        - index.html
    - ...
```
目前可以直接使用[这里](https://github.com/mosquito1803/megalo-web-bundle-demo/blob/master/src/web/index.html)的模板，后续会考虑将模板添加到cli创建的工程中

## 编译配置

1. 入口

对应小程序中app.js/App.vue，在进行web编译时默认会从这两个文件中查找页面配置，如果工程中文件名不同，需要在megalo.config.js中额外配置，否则会因为无法查找到页面配置而出错。如：
```
appEntry: { 
    jsEntry: 'src/main.js',
    vueEntry: 'src/App.vue'
}
```

## 路由

web目前仅支持hash路由，根据在app.js中配置的页面路径，对应的页面即为对应的hash路由，如`pages/index/index`在h5中对应为`#/pages/index/index`

## 跨平台兼容

api相关可以直接使用@megalo/cli，目前已经对大部分能兼容的api进行了h5版本的兼容，详情可以查看[文档](https://megalojs.org/#/api/megalo-api)。

其他的跨平台兼容，可以参考[这里](https://megalojs.org/#/basic/platforms)（使用platform为web即可）。
