# CLI 服务

## 使用命令

在一个 Megalo CLI 项目中，`@megalo/cli-service` 安装了一个名为 `megalo-cli-service` 的命令。你可以在 npm scripts 中以 `megalo-cli-service`访问这个命令。

`@megalo/cli-service` 本身的 api 设计是参照 `@vue/cli3` 来设计的，所以你可能会感到似曾相识，这也是为了尽量减少学习成本。

这是你使用默认 preset 的项目的 `package.json`：

``` json
{
  "scripts": {
    "dev:wechat": "megalo-cli-service serve",
    "build:wechat": "megalo-cli-service build"
  }
}
```

你可以通过 npm 或 Yarn 调用这些 script：

``` bash
npm run dev:wechat
# OR
yarn dev:wechat
```

如果你可以使用 [npx](https://github.com/zkat/npx) (最新版的 npm 应该已经自带)，也可以直接这样调用命令：

``` bash
npx megalo-cli-service serve
```


## megalo-cli-service serve

```
用法：megalo-cli-service serve [options] [entry]

选项：

  --platform    指定要编译的平台 (默认值: wechat), 可选值: wechat、alipay、swan、tt、h5,其中 tt 和 h5 目前还未得到支持
  --mode    指定环境模式 (默认值：development)
```

`megalo-cli-service serve` 命令会启动一个开发服务器，监听更改的文件并自动编译修改内容；不过目前还未实现webpack动态新增entry，如果新增加了页面，需要重新启动编译命令。


## megalo-cli-service build

```
用法：megalo-cli-service build [options] [entry|pattern]

选项：
  --platform    指定要编译的平台 (默认值: wechat), 可选值: wechat、alipay、swan、tt、h5,其中 tt 和 h5 目前还未得到支持
  --mode        指定环境模式 (默认值：production)
  --report      生成 report.html 以帮助分析包内容
  --report-json 生成 report.json 以帮助分析包内容
```

`megalo-cli-service build` 会在 `dist-wechat` 目录产生一个可用于生产环境的包，带有 JS/CSS 的压缩混淆

## 配置时无需 Eject

通过 `sao npm:@megalo/cli your-project-name` 创建的项目无需额外的配置就已经可以跑起来了。绝大多数情况下，你只需要在交互式命令提示中选取需要的功能即可。

不过我们也知道满足每一个需求是不太可能的，而且一个项目的需求也会不断改变。通过 megalo CLI 创建的项目让你无需 eject 就能够配置工具的几乎每个角落。更多细节请查阅[megalo-config-js](cli/megalo-config-js)。
