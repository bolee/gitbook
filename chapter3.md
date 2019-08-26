# 第四章 结构组织

## 一、MVC设计模式

`Apple`默认提供的是`MVC`，对应的`view`为`storyboard`文件，大部分的逻辑都写在`Controller`里面，如果使用代码编写控件的时候会导致`Controller`更加臃肿，可以对控件进行封装减少控制器的负载。

## 二、MVVM设计模式

在业务功能逐渐增多时候，出现了`MVVM`替代`MVC`解放`Controller`部分功能，这其中较常用的是结合`ReactiveObjC`框架进行数据传递，将之前的事件用信号量方式进行替代。可以调整目录结构为

```
ViewController  // 与控制器相关回调事件，导航栏部分
View //视图部分，包括布局
ViewModel // view和model之间的数据处理部分，回调view部分刷新界面，获取view事件拉取model的数据
Model // 包含网络获取数据，缓存数据和对数据操作
```

## 三、目录结构

下面的项目结构具有通用性，根据不同规模显示会有一定的差异，只显示了业务模块部分，并不是工程项目结构，未包含`Pods`、`Frameworks`和`XXXTest`等系统生成的部分。

* AppDelegate -- 系统入口信息，如果功能不断扩展也可以添加分类，如下添加调试信息和推送部分
    * AppDelegate+Debug.h
    * AppDelegate+Debug.m
    * AppDelegate+Push.h
    * AppDelegate+Push.m

* Main(Class) -- 主要业务模块，包含整个系统的业务部分代码，建议以首页模块划分为基准，然后随着业务扩展
    * Base -- 基础部分，包含`BaseView`、`BaseModel`、`BaseController`等基类信息
    * Tabbar -- 具有`UITabBar`项目
    * Home -- 首页信息
    * User -- 用户模块

* Component -- 组件部分，一般是与业务无关的部分封装成模块，如果模块具有更通用性可以跨项目时候，可以考虑提取出去封装为framework模式
    * Cache -- 缓存模块
    * Networking -- 网络模块
    * MapView -- 地图模块

* Category -- 分类，对原有累的功能扩展，如果功能具有明显的不同也可以扩展多个分类，下面只针对不同类在分组，防止目录深度扩展
    * NSString -- 对字符串的扩展，下面的示例显示的是对两个功能进行扩展
        * NSString+init.h
        * NSString+init.m
        * NSString+number.h
        * NSString+number.m

* Service -- 具有摸个独立功能可以单独设计成服务模式，如系统的定位模块、蓝牙模块、加密模块等，

* Vendor -- 第三方框架，虽然大部分都可以用`Cocoapods`和`Carthage`管理，部分厂家`SDK`并未发布到管理平台

* Supporting Files -- 系统默认创建的，包含入口`storyboard`、`Assets`、`Info.plist`文件

* Resource -- 资源文件，图片资源，文件资源，建议按照模块进行划分，有些图片具有公用属性可以单独分类
