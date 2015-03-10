# Objective-C 編碼風格手冊

## 目錄

* [點表示法](#點表示法)
* [空格](#空格)
* [條件語句](#條件語句)

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

or

```objc
if (!error) return success;
```
