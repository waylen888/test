# 點表示法 #應該*僅用於*獲取和改變屬性，中括號表示法用於所有其他實例。  
正確用法：  
`view.backgroundColor = [UIColor orangeColor];       
[UIApplication sharedApplication].delegate;`  
錯誤用法：  
`[view setBackgroundColor:[UIColor orangeColor]];  
UIApplication.sharedApplication.delegate;`  
