# Objective-C 編碼風格手冊

## 目錄

* [點表示法](#點表示法)
* [空格](#空格)
* [條件語句](#條件語句)
* [三元運算子](#三元運算子)
* [方法](#方法)
* [變量](#變量)
* [命名](#命名)
* [下劃線](#下劃線)
* [註釋](#註釋)
* [初始化&內存釋放](#初始化&內存釋放)
* [Literals字面量](#Literals字面量)
* [CGRect函數](#CGRect函數)
* [常量](#常量)
* [枚舉類型](#枚舉類型)
* [私有屬性](#私有屬性)
* [圖片名稱](#圖片名稱)
* [布爾變量](#布爾變量)
* [單例](#單例)
* [Xcode項目](#Xcode項目)

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

## 空行排版

* 函式大括號和其它大括號（比如`if`/`else`/`switch`/`while`等等）在語句的同一行開始，而在新的一行結束。

**正確用法**
```objc
if (user.isHappy) {
    //Do something
}
else {
    //Do something else
}
```

* 每個函式間用一個空白行做間隔，利於閱讀，而不同功能群利用`#pragma mark`加上功能群名稱隔開。
* 在函式宣告中，在(-/+)符號之後加上一個空格。
* `dealloc`函式應放在`@implementation`區塊的頂部，在`@synthesize`和`@dynamic`語句之後。而`init`初始化方法應放在`dealloc`方法之後。

**相關函式結構應該如下**

```objc
@implementation

#pragma mark life-cycle
- (void)dealloc {
    [super dealloc];
}

- (instancetype)init {
    self = [super init];
    if (self) {
        //Do something
    }
    return self;
}

#pragma mark MTKChartViewDelegate
- (void)chartView:(MTKChartView *)chartView didDrawStockItem:(StockItem *)item {
    //Do something
}

@end
```

## 條件語句

為了避免錯誤及提高閱讀性，條件語句必須使用大括號，即使語句體中的語句可以不必使用大括號（比如只有一行語句）。

常見的錯誤包括在不使用大括號的情況下添加第二行語句，以為它屬於`if`語句的一部分。此外，更可怕的事情是，如果條件語句中的代碼行被註釋，則原本第二行語句將變成條件語句的一部分。所以這種編碼風格必須和其它條件語句均保持一致。

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

## 運算式

* 為了讓運算式易讀，運算子間使用空白隔開，且鼓勵多使用括號做優先級別的區隔，例如：
```objc
result = a + ((b - c) / ((d * e) / f)) + g;
```

* 當運算子可以讓代碼顯得更清晰易懂時可使用**三元運算子**。但如果是複雜的運算，應使用類似`if`的條件語句對多種條件進行判斷運算。

**正確用法**
```objc
result = a > b ? x : y;
```

**錯誤用法**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## 變量

* 變量的命名應有意義並具有自解釋性。除了在`for()`循環語句中，應避免使用單個字母變量名稱。
* 除非是常量，星號應緊貼變量名稱表示指向變量的指針，比如正確用法 `NSString *text;` 而非錯誤用法 `NSString* text;` 或 `NSString * text;`
* 應盡可能使用`@property`宣告，避免使用實例變量。

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

* 禁止在初始化函式（`init`、`initWithCoder:`，等等），`dealloc`函式中使用`@property`的存取函式存取變量，例如：

```objc
- (void)dealloc {
    [_count release];
    [super dealloc];
}

- (instancetype)initWithCount:(NSNumber *)startingCount {
    self = [super init];
    if (self) {
        _count = [startingCount copy];
    }
    return self;
}
```

而相反地，在其他地方則使用`@property`存取變量，不要直接存取實例變量，例如：

```objc
- (void)reset {
    self.count = @(0);
}
```

更詳細的說明可以參考[Practical Memory Management](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

#### 變量修飾符號

變量修飾符(`__strong`、`__weak`、`__unsage_unretained`、`__autoreleasing`) 應該放置在指針星號和變量名稱之間，如 `NSString * __weak text`。

命名
====

* 命名規則應盡可能符合Apple的[memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)([NARC](http://stackoverflow.com/a/2865194/340508))。在Objective-C中鼓勵使用長的描述性的方法和變量名稱。

**正確用法**

```objc
UIButton *settingsButton;
```

**錯誤用法**

```objc
UIButton *setBut;
```

* 私有常量名稱在字首加上一個字母`k`，而公開的常量名稱則需冠上公司縮寫字母或其Class的相關名稱作為前綴，並在header文件中使用`extern`，例如：

在.h文件中

```objc
// 公開常量
extern const NSTimeInterval MTKViewControllerNavigationFadeAnimationDuration;
```
在.m文件中

```objc
const NSTimeInterval MTKViewControllerNavigationFadeAnimationDuration = 2.0;
// 私有常量
static const NSTimeInterval kTopViewFadeAnimationDuration = 2.0
```

屬性名稱應使用camel-case（駝峰式）命名方法，第一個字母應為小寫。

> 但對於眾所皆知的名稱縮寫及特定名稱縮寫，無論在變量名稱的任何位置應該永遠保持大寫。
>
> **正確用法**
>
> ```objc
> NSString *URLConnectionString;
> NSArray *HTTPHeaders;
> NSString *userID;
> ```
>
> **錯誤用法**
>
> ```objc
> NSString *urlConnectionString;
> NSArray *httpHeaders;
> NSString *userId;
> ```

屬性對應的實例變量名稱的第一個字母應為小寫，且在前面加上**下劃線**（**如果LLVM版本支持對變量的自動合成，則不必使用**）。

**正確用法**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**錯誤用法**

```objc
id varnm;
```

##註釋

在需要註釋的地方，應使用註釋來解釋某一塊特定的代碼的功能。所有的代碼註釋必須是最新的，非必要的註釋就刪掉，盡量使用行註釋，而避免使用塊註釋。

無用的代碼就直接刪掉，而不要只是註釋，除非該段代碼有對照用途或其他特殊需求。

## Literals字面量

在宣告`NSString`、`NSDictionary`、`NSArray`和`NSNumber`等**immutable**的實例對象時，應使用字面量。需要註意的是`nil`不可傳遞給`NSArray`和`NSDictionary`字面量，否則會造成Crash。

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

## CGRect函數

當需要獲取一個 `CGRect` 矩形的 `x`、`y`、`width`、`height` 屬性時，應使用 `CGGeometry` 函數，而非直接訪問結構體成員。

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

對於常量字串、字面量或數字等等，應使用`static`方式宣告，而非使用`#define`的方式來定義。

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

## 枚舉類型

在使用`enum`的時候，使用最新SDK所包含的巨集`NS_ENUM()`，因為它具備更強的類型檢查和代碼自動完成功能。

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

## 私有屬性
 
私有`@property`應在`.m`文件的Class擴展（匿名分類）中進行宣告，而避免使用命名分類（比如`NYTPrivate`或`private`）。

匿名分類宣告結構如下：

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```


## 布爾變量

因為`nil`將被解析為`NO`，因此沒有必要在條件語句中進行比較。永遠不要將任何東西和`YES`進行直接比較，因為`YES`被定義為1，而一個`BOOL`變量可以有8個bits。

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

如果一個`BOOL`屬性使用形容詞來表達，屬性將忽略"is"前綴，但會強調慣用名稱，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

命名方式可參考[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)。

## 單例

在創建單例對象的共享實例時，應使用線程安全模式。

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

## Xcode項目相關

為避免文件混亂，實體文件應和Xcode項目保持一致。在Xcode中所創建的任何group都應有實體文件系統中相對應的文件夾。不應僅根據文件類型來進行分組，還需要考慮到其作用。

在Xcode的target的Build Setting中，中盡量開啟”Treat Warnings as Errors“，同時盡量開啟[其他的警告](http://boredzo.org/blog/archives/2009-11-07/warnings)。

如果需要忽略某個特定的警告，可以使用[Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)。


