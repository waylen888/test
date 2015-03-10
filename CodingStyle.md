# Objective-C 編碼風格手冊

## 目錄
* [Dot-Notation Syntax](#dot-notation-syntax)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
## Dot-Notation Syntax
應該*僅用於*獲取和改變屬性，中括號表示法用於所有其他實例。  
正確用法：  

	view.backgroundColor = [UIColor orangeColor];
	[UIApplication sharedApplication].delegate;

錯誤用法：  

	[view setBackgroundColor:[UIColor orangeColor]]; 
	UIApplication.sharedApplication.delegate;

This is [an example][tag2] reference-style link.



























這裡是連結


































[tag2]:這裡是連結二
