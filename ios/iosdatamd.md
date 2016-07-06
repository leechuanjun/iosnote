# iOS应用数据存储的常用方式

##iOS应用数据存储的常用方式

 - #### 1、XML属性列表（plist）归档
 - #### 2、Preference(偏好设置)
 - #### 4、SQLite3 
 - #### 5、Core Data

###应用沙盒
- 每个iOS应用都有自己的应用沙盒(应用沙盒就是文件系统目录)，与其他文件系统隔离。应用必须待在自己的沙盒里，其他应用不能访问该沙盒


 ![](save.png)
 
应用程序包：(上图中的Layer)包含了所有的资源文件和可执行文件
- Documents：保存应用运行时生成的需要持久化的数据，iTunes同步设备时会备份该目录。例如，游戏应用可将游戏存档保存在该目录

- tmp：保存应用运行时所需的临时数据，使用完毕后再将相应的文件从该目录删除。应用没有运行时，系统也可能会清除该目录下的文件。iTunes同步设备时不会备份该目录

- Library/Caches：保存应用运行时生成的需要持久化的数据，iTunes同步设备时不会备份该目录。一般存储体积大、不需要备份的非重要数据

- Library/Preference：保存应用的所有偏好设置，iOS的Settings(设置)应用会在该目录中查找应用的设置信息。iTunes同步设备时会备份该目录

##应用沙盒目录的常见获取方式

###Documents：(2种方式)
利用沙盒根目录拼接”Documents”字符串

##方法一
```objc
// 不建议采用，因为新版本的操作系统可能会修改目录名
NSString *home = NSHomeDirectory();
NSString *documents = [home stringByAppendingPathComponent:@"Documents"];

```
##方法二
利用NSSearchPathForDirectoriesInDomains函数
// NSUserDomainMask 代表从用户文件夹下找
// YES 代表展开路径中的波浪字符“~”（即系绝对路径）
```objc
NSArray *array =  NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, NO);
// 在iOS中，只有一个目录跟传入的参数匹配，所以这个集合里面只有一个元素
NSString *documents = [array objectAtIndex:0];
```objc

###Library/Caches：
```objc
    NSArray *arr = @[@"123",@"fff"];
    
    // 获取Cache文件路径
    // NSSearchPathDirectory:搜索的目录
    // NSSearchPathDomainMask：搜索范围 NSUserDomainMask:表示在用户的手机上查找
    // expandTilde 是否展开全路径，如果没有展开，应用的沙盒路径就是~
    // 存储一定要要展开路径
    NSString *cachePaht = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    
    // 拼接文件名
    NSString *filePath = [cachePaht stringByAppendingPathComponent:@"personArr.plist"]; 
   
    NSLog(@"%@",cachePaht);
    
    // File:文件的全路径
    [arr writeToFile:filePath atomically:YES];
```

###temp

```objc
 NSString *tmp = NSTemporaryDirectory();
```
