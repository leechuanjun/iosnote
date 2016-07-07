# 单例模式

###单例模式的作用
- 可以保证在程序运行过程，一个类只有一个实例


###单例模式的使用场合
- 在整个应用程序中，共享一份资源（这份资源只需要创建初始化1次）


##方法一
```objc
#import "XMGPerson.h"

@interface XMGPerson() <NSCopying>

@end

@implementation XMGPerson

static XMGPerson *_person;

+ (instancetype)allocWithZone:(struct _NSZone *)zone
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _person = [super allocWithZone:zone];
    });
    return _person;
}

+ (instancetype)sharedPerson
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _person = [[self alloc] init];
    });
    return _person;
}

- (id)copyWithZone:(NSZone *)zone
{
    return _person;
}
@end
```

##方法二
```objc
static id _instance;

//重写allocWithZone:方法，在这里创建唯一的实例（注意线程安全）
+ (id)allocWithZone:(struct _NSZone *)zone
{
    @synchronized(self) {
        if (!_instance) {
            _instance = [super allocWithZone:zone];
        }
    }
    return _instance;
}

//提供1个类方法让外界访问唯一的实例
+ (instancetype)sharedSoundTool
{
    @synchronized(self) {
        if (!_instance) {
            _instance = [[self alloc] init];
        }
    }
    return _instance;
}
//实现copyWithZone:方法
- (id)copyWithZone:(struct _NSZone *)zone
{
    return _instance;
}
```

