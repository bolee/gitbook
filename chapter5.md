# 第六章 第三方框架

第三方包管理工具有`Cocoapods`和`Carthage`，`swift`自3.0以后开始自带`Package Manager`，直接连接到开源框架地址即可，示例如下：

```
// swift-tools-version:4.0
import PackageDescription

let package = Package(
    name: "DeckOfPlayingCards",
    products: [
        .library(name: "DeckOfPlayingCards", targets: ["DeckOfPlayingCards"]),
    ],
    dependencies: [
        .package(url: "https://github.com/apple/example-package-fisheryates.git", from: "2.0.0"),
        .package(url: "https://github.com/apple/example-package-playingcard.git", from: "3.0.0"),
    ],
    targets: [
        .target(
            name: "DeckOfPlayingCards",
            dependencies: ["FisherYates", "PlayingCard"]),
        .testTarget(
            name: "DeckOfPlayingCardsTests",
            dependencies: ["DeckOfPlayingCards"]),
    ]
)
```

## 一、Cocoapods管理

系统安装`Cocoapods`，可以去官网[下载](https://cocoapods.org/)或使用`sudo gem install cocoapods`管理工具下载，初始化项目：
```
pod init
```
会生成`.xcworkspace`文件，包含该项目`project`和`Pods.project`，通过`framework`模式导入到项目里面，`Podfile`管理第三方包列表，可以配置不同模式和不同项目的依赖包，可以指定不同版本和自定义仓库地址。使用`pod install`安装，`pod update`进行更新。


## 二、Carthage管理

可以通过`brew`和`port`管理工具下载，也可以直接下载编译好的安装文件`.pkg`和源码进行安装，安装文档[地址](https://github.com/Carthage/Carthage#installing-carthage)。在项目目录创建文件`Carthage`，文件内容：

```
github "Alamofire/Alamofire" ~> 4.7.2
```
安装命令：`carthage update`，会将仓库源码`checkout`下来并编译成`framework`。

## 三、自定义框架

如果自定义模块具有通用性，可以作为第三方框架模式引入到项目中，可以直接指定仓库地址或自建`cocoapods`的`spec`仓库进行发布，具体可以参考官方[文档](https://guides.cocoapods.org/making/private-cocoapods.html)。

## 四、常见开源框架

`Objective0-C`：

* AFNetworking：网络框架
* SVProgressHUD：进度条hud
* MBProgressHUD：进度条hud
* YYKit：工具集合
* IQKeyboardManager：键盘管理工具
* Masonry：AutoLayout布局框架
* FMDB：sqilt数据库框架
* ReactiveObjC：Reactive的OC版本
* MJRefresh：下拉刷新
* FLEX：Flipboard出品的调试工具，hook到网络，界面，磁盘数据，界面查看
* Reveal-SDK：界面调试工具，付费软件
* LookinServer：微信出品的界面调试工具，免费
* SDWebImage：图片工具，包括缓存
* CocoaLumberjack：日志框架

`Swift`：
* Snapkit：AutoLayout布局框架
* Alamofire：网络请求框架
