# 第一章 编程规范

## 一、命名规范

1. 驼峰式命名方法。变量名，方法名都必须遵循，以小写字母开头，英文单词进行拼接，首字母大写。宏定义为全大写，常量大写开头。但专有名字可以全部大写，不受限制，如`HTTP`、`TCP`、`UDP`等，只有表示专有名称时候才有效。

2. 命名必须具有意义。尽量避免缩写，Objective-C语言官方提供的标准是行为动词加名词进行组合为方法名称，变量名为表示的意义单词组合起来，添加行为动词组合后可以通顺阅读并能理解含义。

3. 前缀。如果过需要添加模块或项目标识，可以添加前缀，如`SDWebImage`框架的前缀是`sd_`，用小写加下划线`_`，`IQKeyboardManager`框架的前缀为`IQ`，采用两个缩写大写字母标记。

4. 不要与系统框架保留字冲突，如`CG`、`NS`、`UI`、`CA`等，这些都是被已知的系统框架，即使扩展系统框架也需要与系统的区分。

5. 尽量不要使用下划线做前缀，系统私有函数都是单(`_`)或双(`__`)下划线，可能会导致不可预见的逻辑甚至严重的`bug`。

6. 分类命名遵循变量命名规则，建议添加前缀以作区分。

```

// 正确示范
@property (nonatomic, strong) NSString * title;

// 错误示范
@property (nonatomic, strong) NSString * Date;

```

## 二、命名规则

1. 变量命名必须具有意义，规避无意义的变量名，如`i`、`j`、`k`等，只有在临时作用域内的临时变量可以使用，不能影响到阅读，一般只有简单的逻辑才可以使用。

2. 常量一般可以分为两种，全局使用和模块内。全局常量，一般是整个APP系统需要使用的，如当前环境变量，系统配置信息。模块内，使用常量供当前内部使用或对外暴露的变量，如`NSNotification`的名称。如果是全局的可以用单独文件，业务较多可以进行拆分成多个。

3. 宏一般定义为基础数据类型，与常量区别是宏仅仅做替换，没有内存分配。比如获取当前一些界面布局尺寸，分页数据大小，线程总数等等，大部分宏定义目的是与一些编译相关的信息。

4. 方法遵循表义功能，通过其名可以清楚了解其含义，常用行为动词有`set`、`get`、`add`、`remove`等。

5. 需要供外部使用方法和变量才写到`.h`文件，并且添加必要的注释，表达使用方法，在必要场景还需要说明可能出现的异常信息和特定状况下的使用说明。其他内部使用属性和方法都放在`.m`文件内。

## 三、书写规范

1. 每一行的长度设置为80(直接在`Xcode`里面设置`Text Editing` -> `Editing` -> `Page guide at column`)，便于阅读，在大部分计算机编程s语言里面设置行长度为80，过长会导致阅读不方便，可以使用换行。

2. 函数参数过多时候以`:`对齐左右`k`和`v`进行分行，在调用时候时候也可以遵循，`Xcode`目前都是支持直接空格换行对齐。

3. 大括号对齐`{`和`}`，在`Objective-C`里面建议是不换行，在`if`里面即使只有一行也要使用`{}`将内容包含，防止扩展时候出现条件遗漏导致逻辑被更改。

4. 集合类`NSArray`和`NSDictionary`在添加时候需要空格。

```
NSDictionary * dict = @{ @"k": @"v", @"k1": @"v1"};
NSArray * list = @[ @{"k": @"v"}, @{@"k1": @"v1"}];
```

5. 空属性修饰。目前有三种方式可以修饰是否为空，可以为空`nullable`、`__nullable`、`_Nullable`，不允许为空`nonnull`、`__nonnull`、`_Nonnull`，示例如下：

```
@property (nonatomic, strong, nullable) NSString * title;
@property (nonatomic, strong) NSString * __nullable title;
@property (nonatomic, strong) NSString * _Nullable title;
```

其中`nullable`/`nonull`用法不一样(如果是双指针、`block`的返回值和参数则不能使用)，`__nullable`/`__nonnull`已经被最新的`_Nullable`/`_Nonnull`替代，在一些旧的项目里还存在。三者功能都是一样的，官方提供了两个宏可以直接指定默认为`nonnull`：

```
NS_ASSUME_NONNULL_BEGIN

@interface myClass ()

@property (nonatomic, copy) NSString *aString;

- (id)methodWithString:(nullable NSString *)str;

@end

NS_ASSUME_NONNULL_END
```

6. 判空处理。尽量在使用数据时候添加分类方法去判断，再调用系统方法，不建议直接使用`runtime`进行拦截处理。常见的有`NSArray`、`NSDictionary`、`NSAttributeString`进行存取操作时候都不允许为空，系统函数并未做判空处理，可以将对应的方法添加到分类做统一处理。

7. 常量命名。数字类型的使用枚举类型`NS_ENUM`和`NS_OPTIONS`，不要使用`1.2.3`去比较判断。其中`NS_OPTIONS`使用的是`bitmask`，使用`&`和`|`进行判断，示例：

```
typedef NS_OPTIONS(NSUInteger, SDWebImageDownloaderOptions) {
    SDWebImageDownloaderLowPriority = 1 << 0,
    SDWebImageDownloaderProgressiveLoad = 1 << 1,
    SDWebImageDownloaderUseNSURLCache = 1 << 2,
}
// how to use
SDWebImageDownloaderOptions option;
if (option & SDWebImageDownloaderUseNSURLCache) {
    // other 
}
```

8. `KVO`和`NSNotification`必须主动清除。注入添加了对象观察或监听，必须手动清除，否则观察者销毁后，发送者会调用已销毁对象导致崩溃。

9. `BOOL`类型判断，不要直接使用`YES == isGet`进行判断，`BOOL`定义的是`signed char`类型，`YES`、`NO`定义的是1和0，如下示例：

```
    int temp = 2;
    if (temp == YES) {
        NSLog(@"2 is YES");
    } else {
        NSLog(@"2 is NO");
    }
```

输出结果为`2 is NO`，很显然不是我们想要的结果。

10. `CoreFoundation`和`CoreGraphics`框架的内存需要手动处理，对应的申请和释放接口都有提供，与`C`相关的部分内存不归`Objective-C`管理，所以`ARC`无效，并且与`Objective-C`赋值时候需要桥接(`bridge`)模式。


## 四、注释

1. 文档必须要有注释。如果有特殊场景或异常状况都需要写在文档说明里面，因业务扩展某些相似功能的页面因为命名问题可能导致含糊不清，如果没有文档说明很难区分具体调用场景。

2. 属性注释，因为命名规范已经可以体现含义，只需要简单说明即可，在特定场景或异常则需要另外添加说明。

3. 方法注释，简短说明作用，特殊场景或异常信息需要给出说明

4. 常见注释参数，一些工具可以自动生成对应的文档，可以查看官方[文档](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/HeaderDoc/intro/intro.html)更详细说明，虽然较长时间没有更新，依然具有一定的参考意义。

* @brief：简短解释说明
* @discussion：详细的描述
* @param：参数说明
* @return：返回值说明
* @see：查看参考的字段或方法说明
* @sa：与see一样
* @code：嵌入示例代码
* @remark：附加的一些特殊说明
