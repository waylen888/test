# Objective-C 編碼風格手冊

## 目錄

* [點表示法](#點表示法)
* [空格](#空格)
* [條件語句](#條件語句)
* [三元运算子](#三元运算子)
* [方法](#方法)
* [变量](#变量)
* [命名](#命名)
* [下划线](#下划线)
* [注释](#注释)
* [初始化&内存释放](#初始化&内存释放)
* [Literals字面量](#Literals字面量)
* [CGRect函数](#CGRect函数)
* [常量](#常量)
* [枚举类型](#枚举类型)
* [私有属性](#私有属性)
* [图片名称](#图片名称)
* [布尔变量](#布尔变量)
* [单例](#单例)
* [Xcode项目](#Xcode项目)

## 點表示法

應該**僅用於**獲取和改變屬性，中括號表示法用於所有其他實例。

**正確用法**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```
**錯誤用法**
```
[view setBackgroundColor:[UIColor orangeColor]]; 
UIApplication.sharedApplication.delegate;
```
## 空格

* 行缩进使用4个空格。禁止使用Tab键来缩进。请在Xcode偏好设置中进行设置。
* 方法大括号和其它大括号（比如`if`/`else`/`switch`/`while`等等）应在语句的同一行开始，而在新的一行关闭。

**正確用法**
```objc
if (user.isHappy) {
//Do something
}
else {
//Do something else
}
```
* 为保证视觉上的整洁和代码组织，在方法之间应提供且仅提供一行空白。方法中的空白应用于区分功能，但空白行最好用于区分两个不同方法。
* `@synthesize`和`@dynamic`应在方法实现的新一行中声明。

## 條件語句

为避免错误，条件语句体必须使用大括号，即便语句体中的语句可以不必使用大括号（比如只有一行语句）。常见的错误包括在不使用大括号的情况下添加第二行语句，以为它属于if语句的一部分。此外，更可怕的事情是，如果条件语句中的代码行被注释，则本不术语条件语句的下一行代码将变成条件语句的一部分。此外，这种编码风格和所有其它条件语句均保持一致。

**正確用法**
```objc
if (!error) {
    return success;
}
```

**錯誤用法**
```objc
if (!error)
    return success;
```

或

```objc
if (!error) return success;
```

## 三元运算子

仅当使用该运算子可以让代码显得更清晰易懂时方可使用三元运算子。更多情况下应使用条件语句。使用类似`if`的条件语句对多种条件进行判断通常要更容易理解，或使用实例变量。

**正確用法**
```objc
result = a > b ? x : y;
```

**錯誤用法**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## 方法

在方法宣告中，在(-/+)符号之后应加上一个空格。此外，在方法段之间应添加一个空格。

**正確用法**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 變量

变量的命名应尽可能具有自解释性。除了在`for()`循环语句中，应避免使用单个字母变量名称。
除非是常量，星号应紧贴变量名称表示指向变量的指针，比如正確用法 `NSString *text;` 而非錯誤用法 `NSString* text;` 或 `NSString * text;`
应尽可能使用属性定义替代单一的实例变量。避免在初始化方法,`dealloc`方法和自定义的setter和getter方法中直接读取实例变量参数（`init`,`initWithCoder:`，等等）。
更多信息请参看[這裡](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

**正確用法**
```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**錯誤用法**
```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### 變量修飾符號

變量修飾符(`__strong`、`__weak`、`__unsage_unretained`、`__autoreleasing`) 應該放置在指針星號和變量名稱之間，如 `NSString * __weak text`。

## 命名

苹果的命名规范应尽可能符合[内存管理法则](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)([NARC](http://stackoverflow.com/a/2865194/340508))。

在Objective-C中鼓励使用长的描述性的方法和变量名称。

**正確用法**

```objc
UIButton *settingsButton;
```

**錯誤用法**

```objc
UIButton *setBut;
```

对于类和常量名称，应尽量使用三大写字母前缀（比如NYT），但对Core Data的实体名称可不适用该法则。

常量名称应将其中的所有单词的首字母大写，同时加上相关类的名称作为前缀。

**正確用法**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**錯誤用法**

```objc
static const NSTimeInterval fadetime = 1.7;
```

属性名称应使用camel-case（驼峰式）命名方法，且第一个单词的首字母应为小写。

属性对应的实例变量名称的第一个单词的首字母应为小写，且在前面加上下划线。**如果LLVM版本支持对变量的自动合成，则不必使用。**

**正確用法**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**錯誤用法**

```objc
id varnm;
```

對於眾所皆知的名稱縮寫及特定名稱縮寫，無論在變量名稱的任何位置應該永遠保持大寫。

**正確用法**

```objc
NSString *URLConnectionString;
NSArray *HTTPHeaders;
NSString *userID;
```

**錯誤用法**

```objc
NSString *urlConnectionString;
NSArray *httpHeaders;
NSString *userId;
```

## 注释

在需要注释的地方，应使用注释来解释某一块特定的代码的功能。所有的代码注释必须是最新的，要吗就删掉。
应尽量使用行注释，而避免使用块注释。之所以这样是因为代码自身需要是自文档化的，因此只需要零散添加一些行注释。当然，对于用于生成文档的注释，该原则并不适用。

## 初始化&内存释放

`dealloc`方法应放在方法实现文件的顶部，在`@synthesize`和`@dynamic`语句之后。`init`初始化方法应放在`dealloc`方法之后。

`init` 方法結構應該如下：

```objc
- (void)dealloc {
    [super dealloc];
}

- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }
    return self;
}
```

## Literals字面量

在创建`NSString`、`NSDictionary`、`NSArray`和`NSNumber`等对象的immutable实例时，应使用字面量。需要注意的是`nil`不可传递给NSArray和NSDictionary字面量，否则会引起程序崩溃。

**正確用法**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**錯誤用法**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect函数

当需要获取一个 `CGRect` 矩形的 `x`、`y`、`width`、`height` 属性时，应使用 `CGGeometry` 函数，而非直接访问结构体成员。

**正確用法**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**錯誤用法**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## 常量

相对字符串字面量或数字，我们更推荐适用常量。应使用`static`方式声明常量，而非使用`#define`的方式来定义宏。

**正確用法**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**錯誤用法**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## 枚举类型

在使用`enum`的时候，使用最新SDK所包含的巨集`NS_ENUM()`，因为它具备更强的类型检查和代码完成功能。

宣告的結構如下：

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

#### 位元遮罩

如在`enum`裡使用位元遮罩，在宣告時使用`NS_OPTIONS`巨集。

宣告的結構如下：

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
  NYTAdCategoryAutos      = 1 << 0,
  NYTAdCategoryJobs       = 1 << 1,
  NYTAdCategoryRealState  = 1 << 2,
  NYTAdCategoryTechnology = 1 << 3
};
```

## 私有属性
 
私有属性应在类实现文件的类扩展（匿名分类）中进行声明。应避免使用命名分类（比如`NYTPrivate`或`private`）。

匿名分類宣告結構如下：

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```


## 布尔变量

因为`nil`将被解析为`NO`，因此没有必要在条件语句中进行比较。永远不要将任何东西和`YES`进行直接比较，因为`YES`被定义为1，而一个`BOOL`变量可以有8个字节。

**正確用法**

```objc
if (!someObject) {
}
```

**錯誤用法**

```objc
if (someObject == nil) {
}
```

-----

**以下是`BOOL`變量的使用**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**錯誤用法**

```objc
if (isAwesome == YES) // Never do this.
if ([someObject boolValue] == NO)
```

-----

如果一个`BOOL`属性使用形容词来表达，属性将忽略"is"前缀，但会强调惯用名称，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

命名方式可參考[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)。

## 单例

在创建单例对象的共享实例时，应使用线程安全模式。

```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```

使用這個方法可以避免[不可預知的問題](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)。

## Xcode项目

为避免文件混乱，实际的物理文件应和Xcode项目保持一直。在Xcode中所创建的任何group都应有文件系统中相对应的文件夹。不应仅根据文件类型来进行分组，还需要考虑到其作用。

在Xcode的target的Build Setting中，中尽量开启”Treat Warnings as Errors“，同时尽量开启[其他的警告](http://boredzo.org/blog/archives/2009-11-07/warnings)。

如果需要忽略某个特定的警告，可以使用[Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)。



