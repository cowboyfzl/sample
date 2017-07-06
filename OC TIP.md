# sample
1.OC与C++和java最大区别在于OC语言使用的”消息结构”模式，而另外两种是“函数调用”模式，前者是运行时执行代码的环境来决定的，后者是编译器决定的

1.1.OC是C的超集，重点是要了解C语言的内存模型和指针,OC引用计数 
NSString *someString = @“The String”;
指的是一个someString的变量，指向一个NSString的指针
理解深拷贝与浅拷贝，堆内存和栈内存

2.少引用其他头文件不需要知道这个类细节的时候直接@class声明就可以了同时可以避免循环引用，降低耦合

2.1. 无法使用声明的时候比如要遵循某个类的协议,尽量把该类的声明移至“分类”中,不行的话就把协议单独放在一个头文件中，然后将其引入


3.多用字面量语法 (@“糖语法”)，但是要注意在向数组或者字典键值对赋值的时候不要把nil传进去，否则会丢失对象，或者抛出异常

4.多用类型常量，少用预处理#define，，因为预处理无法知道该常量的类型信息 多用static const /*….*/ 这样可以清楚的描述类型信息(比如NSString ,NSTimeInterval)以便于阅读
在实现文件内通常以k开头来命名常量，若可见通常以类名作为前缀
如果设置一个外界可见的常量值需.h中写extern NSString *const+常量名，然后在实现文件中给它赋值 NSString *const +常量名 = 赋值内容 (常量是一个指针，指向一个NSString对象，extern 作用是把常量添加到全局符号表中)


5.如果把传递某个方法的选项表示为枚举类型，而多个选项可以同时使用，那么就将个选项定义为2的幂，以便通过按位或操作组合起来:
  typedef NS_OPTIONS (NSUInteger,EOCConnectDirection){
        EOCConnectDirectionUp = 1<<0,
        EOCConnectDirectionLeft = 1<<1,
        EOCConnectDirectionDown = 1<<2,
        EOCConnectDirectionRight = 1<<3,
    };

如果是不需要组合
 typedef NS_ENUM (NSUInteger, EOCConnectionState){
        EOCConnectionStateDisconnected,
        EOCConnectionStateConnecting,
        EOCConnectionStateConnected,
    };

用NS_OPTIONS或者NS_ENUM宏来定义枚举类型，并指明底层数据类型，这样做可以确保枚举是用底层数据实现出来的，而不会采用编译器所选的类型，在处理枚举类型的switch语句中不要实现default分支，这样的话如果加入新枚举后，编译器会提示开发者:switch语句并未处理所以枚举

6.OC中不尽量不要使用public和@private来区分公共方法和私有方法(这是C++和Java中经常用的)因为这个可能会读取到错误的值尽量使用@property 使用点语法会直接调用属性的set方法和get方法。使用属性，编译器不仅会给它自动合成所需方法还会生成一个有_的实例变量名，这个名字可以通过@synthesize来改变。使用@dynamic可以阻止编译器自动合成属性所需方法

6.1.设置属性时尽量用self.语法来设置其参数，而读取的时候尽量是用_来直接访问实例变量，_可以跳过set,get方法的生成直接给实例变量赋值，速度快。如果用_访问实例变量不会触发KVO通知，初始化方法中应该用直接访问实例变量来赋值，不然可能会被覆写，在dealloc中也同样如此


7.==是比较两个指针本身，而不是对象，isEqual才是比较两个对象的，，，而hash值比较两个对象，即便是返回同一个值，那么isEqual方法返回的值也不一定为真
计算hash码办法
- (NSUInteger)hash{
NSUInteger firstNameHash = [_firstName hash];
NSUInteger lastNameHash = [_lastName hash];
NSUInteger ageHash = _age;
return firstNameHash ^ lastNameHash ^ gaeHash;
}
这种做法既能保持高效又能使生成的hash码少于一定范围

NSException raise:@“” format@“” 是让程序抛出异常的方法

8.可以通过关联对象机制把两个对象联系在一起，定义关联对象时可指定内存管理语义，用以模仿定义属性时采用的“拥有关系”和”非拥有关系”.只有在其他做法不可行时才应选用关联对象，因为这种做法通常会引入难于查找的bug.
方法:获取关联:objc_getAssociatedObject(<#id object#>, <#const void *key#>)
     设置关联:objc_setAssociatedObject(<#id object#>, <#const void *key#>, <#id value#>, <#objc_AssociationPolicy policy#>)
     移除关联:objc_removeAssociatedObjects(<#id object#>)
