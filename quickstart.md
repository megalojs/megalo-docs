# 快速构建

megalo 封装了一个用于快速构建的脚手架，如果你想对现有项目进行改造，可以直接 [DIY webpack 配置](quickstart/webpack)

## 安装 CLI

```shell
$ npm install -g @megalo/cli
```

> - 需要管理员权限
> 
> - 如果安装失败或者过慢，可以切换 [taobao 源](http://npm.taobao.org/)
> ```shell
> npm set registry https://registry.npm.taobao.org/
> ```

## 创建 Megalo 工程

```shell
$ megalo my-megalo-pro

# 或者在已有目录下初始化

$ mkdir my-megalo-pro
$ cd my-megalo-pro
$ megalo
```

## 构建小程序

```shell
# 微信小程序
$ npm run dev:wechat

# 支付宝小程序
$ npm run dev:alipay

# 百度智能小程序
$ npm run dev:swan
```

> 构建结束会在根目录生成相应的小程序包，`/dist-wechat`、`/dist-alipay`、`/dist-swan`，且 watch 文件变化进行热更

## 调试

开启微信开发者工具，将项目目录指向 `/dist-wechat`，填写 AppID 即可执行。

<img src="../static/imgs/init-1.jpg" width="300" style="box-shadow:0 0 10px #666"> 

[*AppID 获取方式*](https://developers.weixin.qq.com/miniprogram/dev/#%E7%94%B3%E8%AF%B7%E5%B8%90%E5%8F%B7)

<img src="../static/imgs/init-2.jpg" width="600" style="box-shadow:0 0 10px #666">

## 然后就可以随便搞了