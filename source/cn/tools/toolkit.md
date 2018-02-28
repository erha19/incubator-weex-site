---
title: 使用 weex-toolkit
type: tools
order: 5.1
version: 2.1
---

# weex-toolkit

[weex-toolkit](https://github.com/weexteam/weex-toolkit) 是官方提供的一个脚手架命令行工具，你可以使用它进行 Weex 项目的创建，调试以及打包等功能。

## 安装

``` bash
$ npm install -g weex-toolkit
```
如果你本地没有安装 node.js 你可以前往[官网](https://nodejs.org/en/)下载安装。

* 请确保你的 node 版本是>=6，你可以使用 [n](https://github.com/tj/n) 来进行 node 的版本管理。

中国用户如果npm遭遇网络问题，可以使用淘宝的 [npm镜像](https://npm.taobao.org/)或通过[nrm](https://www.npmjs.com/package/nrm)工具切换你的npm镜像：

``` bash
$ npm install weex-toolkit -g --registry=https://registry.npm.taobao.org
// 或者
$ nrm use taobao
$ npm install weex-toolkit -g
```

如果你安装的过程中遇到了问题，你可以在[weex-toolkit问题汇总](https://github.com/weexteam/weex-toolkit#faq)中找到解决方法或者在[weex-toolkit issues](https://github.com/weexteam/weex-toolkit/issues)中与我们讨论。


## 命令

### create
```bash
$ weex create awesome-project
```
该命令用于创建一个新的weex项目。命令运行后，你可以找到`awesome-project`目录，里面有一些Weex模板,里面提供了一些内置的脚本

- `build`: 用于打包源文件生成JS bundle
- `dev`: 运行监听模式的webpack打包脚本
- `serve`: 启动热更新静态服务器

你需要在运行`npm start`前运行一下`npm i`来安装项目依赖，之后浏览器就会自动打开开发页面。

### preview

weex-toolkit supports previewing your Weex file(`.we` or `.vue`) in a watch mode. You only need specify your file path.
weex-toolkit工具支持对你的Weex文件（`.vue`)在监听模式下进行预览，你只需要指定一下你的项目路径。

``` bash
$ weex preview src/foo.vue
```

浏览器会自动得打开预览页面并且你可以看到你的weex页面的布局和效果。如果你在你的设备上安装了[Playground](https://weex.apache.org/cn/playground.html)，你还可以通过扫描页面上的二维码来查看页面。

使用下面的命令，你将可以预览整个文件夹中的`.vue`文件

``` bash
$ weex preview src --entry src/foo.vue
```

你需要指定要预览的文件夹路径以及入口文件（通过`--entry`传入）。

### compile

使用 `weex compile` 命令可以编译单个weex文件或者整个文件夹中的weex文件。

``` bash
$ weex compile [source] [dist]  [options]
```

#### 参数

| Option        | Description    | 
| --------   | :-----   |
|`-w, --watch`        | watch we file changes auto build them and refresh debugger page! [default `true`]|
|`-d,--devtool [devtool]`        |set webpack devtool mode|
|`-e,--ext [ext]`        | set enabled extname for compiler default is vue|we|
|`-m, --min`| set jsbundle uglify or not. [default `false`]|

你可以这样子使用：

``` bash
$ weex compile src dest --devtool source-map -m
```

### platform

使用`weex platform [add|remove] [ios|android]`命令可以添加或移除ios/android项目模板。

``` bash
$ weex platform add ios
$ weex platform remove ios
```

使用 `weex platform list`来查看你的项目中支持的平台。

### run

你可以使用`weex-toolkit`来运行android/ios/web项目.

``` bash
$ weex run ios
$ weex run android
$ weex run web
```

### debug

从 `weex-toolkit v1.2.0+` 版本开始，默认的debugger工具从[Weex devtools](https://github.com/weexteam/weex-devtool)切换至[Weex debugger](https://github.com/weexteam/weex-debugger)
** [Weex debugger](https://github.com/weexteam/weex-debugger) ** 是实现[Chrome调试协议](https://developer.chrome.com/devtools/docs/debugger-protocol)的Weex自定义开发工具, 
主要用于帮助你快速检查您的应用程序，并在Chrome网页中调试您的JS bundle源代码，支持Android和iOS平台。所以你可以通过`weex-toolkit`使用的`weex-devtool`功能。

如果你想使用旧版本的devtool工具，可以通过以下命令调用：
```
weex xbind debugx weex-devtool
weex update weex-devtool #如果更新过程报错请删除 `~/.xttolkit/node_modules/weex-devtool`后重新运行
weex debugx
```
#### 用法

``` bash
weex debug [we_file|bundles_dir] [options]
```

#### 参数

| 选项        | 描述    | 
| --------   | :-----   |
|`-v, --version`       | 展示版本信息 |
|`-h, --help`       | 展示帮助界面 |
|`-H --host [host] `   | 设置启动的ip地址（适用于docker等代理环境下）|
|`-p, --port [port]` | 设置启动的端口号 |
|`-m, --manual`   | 开启该选项时debugger将不会自动打开浏览器 |
|`--min`        | 压缩jsbundle |
|`--verbose`|展示详细日志|
|`--loglevel [loglevel]`| 设置日志等级，可选的等级有 silent/error/warn/info/log/debug 默认值为 error |
|`--remotedebugport [remotedebugport]`| 设置调试端口号 默认值为 9222|
|`--telemetry`| 上传用户错误日志帮助提高工具链 |

#### 特性

##### 连接设备

```
$ weex debug
```

该命令会启动一个调试用二维码，有ios模拟器环境的用户可以通过点击二维码在模拟器中进行调试，你也可以使用[Playground](https://weex.apache.org/cn/playground.html)来扫描它来启动调试或者在你的应用中集成[Weex devtools](#集成devtool工具)

![devtools-main](https://img.alicdn.com/tfs/TB1v.PqbmBYBeNjy0FeXXbnmFXa-1886-993.png)

##### 调试`.vue`文件

```
$ weex debug your_weex.vue
```
这个命令会将`your_weex.vue`编译成`your_weex.js`，并根据命令启动调试服务器。
`your_weex.js`将部署在服务器上并显示在调试页面中，使用另一个二维码用于`your_weex.js`文件的调试。

![devtools-entry](https://img.alicdn.com/tfs/TB1j3DIbntYBeNjy1XdXXXXyVXa-1915-1001.png)

##### 调试文件夹中的`.vue`文件

```
$ weex debug your/vue/path
```
这个命令将编译`your/vue/path`中的每个文件，并将它们部署在捆绑服务器上，新的文件将映射到`http://localhost:port/weex/`路径下，同样可以通过上面所示的扫码二维码界面进行查看。

##### Inspector功能
> Inspector功能可查看页面的VDOM/Native Tree结构

注：如不需要此功能尽量保持关闭状态，开启浏览器Inspector界面会增加大量页面通讯，较为影响性能。
![image-toggle](https://img.alicdn.com/tfs/TB166B8bhGYBuNjy0FnXXX5lpXa-2876-1652.png)
![image-inspector](https://img.alicdn.com/tfs/TB11kN2beuSBuNjy1XcXXcYjFXa-2872-1636.png)

##### JS Debug功能
> JS Debug功能可对weex页面中的Jsbundle进行调试。
![image-debug](https://img.alicdn.com/tfs/TB1b5J2beuSBuNjy1XcXXcYjFXa-2880-1648.png)


##### Network功能
> Network功能可以收集weex应用中的网络请求信息。
![image-network](https://img.alicdn.com/tfs/TB126JNbbGYBuNjy0FoXXciBFXa-2868-1642.png)


##### LogLevel和ElementMode功能
> LogLevel和ElementMode功能用于调整调试工具的输出配置。

LogLevel分别有 debug/info/warn/log/error五个log等级，切换可输出不同等级的log信息  
ElementMode可以切换Element标签中Domtree显示模式，下图为vdom显示界面，可从标签中看到详细的数据结构：  
![imgage-loglevel](https://img.alicdn.com/tfs/TB1qzrObntYBeNjy1XdXXXXyVXa-2872-1636.png)

##### Prophet功能（加载时序图）
> Prophet功能用于查看weex的加载时序图和页面性能指标。

点击右上角Prophet即可查看时序图(iOS暂不支持，性能数据可在log的performance中查看)，如下： 

![imgage-prophet](https://img.alicdn.com/tfs/TB1E4rObntYBeNjy1XdXXXXyVXa-2852-1626.png)

##### 集成devtool工具
* Android
    * 查看文档 [Weex devtools (Android)](../../references/advanced/integrate-devtool-to-android.html), 它会引导你一步一步配置和使用它。
* iOS
    * 查看文档 [Weex devtools (iOS)](../../references/advanced/integrate-devtool-to-ios.html), 它会引导你一步一步配置和使用它。
