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

在方法声明中，在(-/ )符号之后应加上一个空格。此外，在方法段之间应添加一个空格。

**正確用法**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 變量

变量的命名应尽可能具有自解释性。除了在`for()`循环语句中，应避免使用单个字母变量名称。
除非是常量，星号应紧贴变量名称表示指向变量的指针，比如正確用法 `NSString \*text;` 而非錯誤用法 `NSString* text;` 或 `NSString * text;`
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


