OC TIP
===

## 1

`OC`与`C++`和`java`最大区别在于`OC`语言使用的**"消息结构"**模式<br>
而另外两种是**"函数调用"**模式<br>
前者是运行时执行代码的环境来决定的，后者是编译器决定的

### 1.1
`OC`是`C`的超集<br>
重点是要了解C语言的内存模型和指针,`OC`引用计数<br>

		NSString *someString = @“The String”;
指的是一个`someString`的变量，指向一个`NSString`的指针<br>
理解[深拷贝与浅拷贝](http://www.jianshu.com/p/e6a7cdcc705d)，[堆内存和栈内存](http://www.jianshu.com/p/c8e1d91dda99)

## 2
少引用其他头文件不需要知道这个类细节的时候直接`@class`声明就可以了同时可以**避免循环引用，降低耦合**

### 2.1
无法使用声明的时候比如要遵循某个类的协议,尽量把该类的声明
移至“分类”中,不行的话就把协议单独放在一个头文件中，然后将其引入

## 3
多用字面量语法 `(@“糖语法”)`，但是要注意在向数组或者字典键值对赋值的时候不要把`nil`传进去，否则会丢失对象，或者抛出异常

## 4
* 多用类型常量，少用预处理`#define`，因为预处理无法知道该常量的类型信息<br>
多用`static const`这样可以清楚的描述类型信息(比如`NSString` ,`NSTimeInterval`)以便于阅读
* 在实现文件内通常以k开头来命名常量，若可见通常以类名作为前缀
如果设置一个外界可见的常量值需.h中写`extern NSString *const + 常量名`然后在实现文件中给它赋值 `NSString *const + 常量名 = 赋值内容` (常量是一个指针，指向一个`NSString`对象，`extern` 作用是把常量添加到全局符号表中)

## 5
如果把传递某个方法的选项表示为枚举类型，而多个选项可以同时使用，那么就将个选项定义为2的幂，以便通过按位或操作组合起来:<br>
  
  	typedef NS_OPTIONS (NSUInteger,EOCConnectDirection){
        EOCConnectDirectionUp = 1<<0,
        EOCConnectDirectionLeft = 1<<1,
        EOCConnectDirectionDown = 1<<2,
        EOCConnectDirectionRight = 1<<3,
    };

如果是不需要组合<br>
 
 	typedef NS_ENUM (NSUInteger, EOCConnectionState){
        EOCConnectionStateDisconnected,
        EOCConnectionStateConnecting,
        EOCConnectionStateConnected,
    };

用`NS_OPTIONS`或者`NS_ENUM`宏来定义枚举类型，并指明底层数据类型，这样做可以确保枚举是用底层数据实现出来的，而不会采用编译器所选的类型，在处理枚举类型的`switch`语句中不要实现`default`分支，这样的话如果加入新枚举后，编译器会提示开发者:`switch`语句并未处理所以枚举

## 6
* `OC`中不尽量不要使用`@public`和`@private`来区分公共方法和私有方法(这是`C++`和`Java`中经常用的，不过`swift`里也需要用了),因为这个可能会读取到错误的值,所以尽量使用`@property` ***使用点语法***会直接调用属性的`set`方法和`get`方法。
* 使用属性，编译器不仅会给它自动合成所需方法还会生成一个有`_`的实例变量名，这个名字可以通过`@synthesize`来改变。使用`@dynamic`可以阻止编译器自动合成属性所需方法

### 6.1
设置属性时尽量用`self.`语法来设置其参数，而读取的时候尽量是用`_`来直接访问实例变量，`_`可以跳过`set`,`get`方法的生成直接给实例变量赋值，***速度快***。如果用`_`访问实例变量***不会触发`KVO`通知***，初始化方法中应该用直接访问实例变量来赋值，不然可能会被覆写，在`dealloc`中也同样如此

## 7
* `==`是比较两个***指针本身，而不是对象***，`isEqual`才是比较两个**对象**的<br>
而[hash](http://www.jianshu.com/p/915356e280fc)值比较两个对象，即便是返回同一个值，那么`isEqual`方法返回的值也不一定为真
计算[hash](http://www.jianshu.com/p/915356e280fc)码办法

		- (NSUInteger)hash{
		NSUInteger firstNameHash = [_firstName hash];
		NSUInteger lastNameHash = [_lastName hash];
		NSUInteger ageHash = _age;
		return firstNameHash ^ lastNameHash ^ gaeHash;
		}
这种做法既能保持高效又能使生成的hash码少于一定范围

* NSException raise:@“” format@“” 是让程序抛出异常的方法

## 8
可以通过***关联对象机制(runTime)***把两个对象联系在一起，定义关联对象时可指定内存管理语义，用以模仿定义属性时采用的“拥有关系”和”非拥有关系”.***只有在其他做法不可行时才应选用关联对象，因为这种做法通常会引入难于查找的bug***.

> 获取关联:****`objc_getAssociatedObject(<id object> <const void *key>)`****<br>
设置关联:****`objc_setAssociatedObject(<id object>, <const void *key>, <id value>, <objc_AssociationPolicy policy>)`****<br>
     移除关联:****`objc_removeAssociatedObjects(<id object>)`****

## 9
理解`objc_msgSend`,首先`C`使用的是静态绑定，编译器就能确定运行时所对应的函数，而`OC`是动态的语言，函数要到运行时才会确定
`OC`一个函数的原型 
		
	void objc_msgSend(id self, SEL cmd, ...)
	
发给某对象的全部消息都要由***动态消息派发***来处理，该系统会查出对应的方法，并执行其代码;消息由接收者，选择子及参数构成。给某个对象发送消息也就相当于在该对象上***调用方法***

## 10
在系统遇到无法解读的消息时就会启动消息转发。消息转发分为两大阶段:

* 先征询接收者，所述的类看能否动态添加方法其名称叫做***动态方法解析***
* ***完整的消息转发机制***第一步接收者看有没有其他对象能够处理这条消息。有则把这个消息转给那个对象，若没有***备援接收者***则启动完成的消息转发机制，第二步运行期系统会把消息有关的全部细节封装到`NSInvocation`对象中，再给接收者最后一次机会<br>

####动态方法解析
`+ (Bool)resolveInstanceMethod:(SEL)selector`如果是类方法则会调用`resolveClassMethod` 

####例1<br>


```
@interface SomeClass : NSObject
@property (assign, nonatomic) float objectTag;
@end

@implementation SomeClass
@dynamic objectTag;  //声明为dynamic
//添加setter实现
void dynamicSetMethod(id self,SEL _cmd,float w){
    printf("dynamicMethod-%s\n",[NSStringFromSelector(_cmd)
                                 cStringUsingEncoding:NSUTF8StringEncoding]);
    printf("%f\n",w);
    objc_setAssociatedObject(self, ObjectTagKey, [NSNumber numberWithFloat:w], OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
//添加getter实现
void dynamicGetMethod(id self,SEL _cmd){
    printf("dynamicMethod-%s\n",[NSStringFromSelector(_cmd)
                                 cStringUsingEncoding:NSUTF8StringEncoding]);
   [objc_getAssociatedObject(self, ObjectTagKey) floatValue];
}

//解析selector方法
+(BOOL) resolveInstanceMethod: (SEL) sel{

    NSString *methodName=NSStringFromSelector(sel);
    BOOL result=NO;
    //动态的添加setter和getter方法
    if ([methodName isEqualToString:@"setObjectTag:"]) {
        class_addMethod([self class], sel, (IMP) dynamicSetMethod,
                        "v@:f");
        result=YES;  
    }else if([methodName isEqualToString:@"objectTag"]){
        class_addMethod([self class], sel, (IMP) dynamicGetMethod,
                        "v@:f");
        result=YES;
    }
    return result;
}
```

####列2：<br>

```
@interface Person : NSObject

@property (nonatomic, copy) NSString* name;
@property (nonatomic, assign) NSUInteger age;

@end

@implementation Person

@synthesize name = _name;
@synthesize age = _age;
//如果需要传参直接在参数列表后面添加就好了
void dynamicAdditionMethodIMP(id self, SEL _cmd) {
    NSLog(@"dynamicAdditionMethodIMP");
}

+ (BOOL)resolveInstanceMethod:(SEL)name {
    NSLog(@"resolveInstanceMethod: %@", NSStringFromSelector(name));
    if (name == @selector(appendString:)) {
        class_addMethod([self class], name, (IMP)dynamicAdditionMethodIMP, "v@:");
        return YES;
    }
    return [super resolveInstanceMethod:name];
}

+ (BOOL)resolveClassMethod:(SEL)name {
    NSLog(@"resolveClassMethod %@", NSStringFromSelector(name));
    return [super resolveClassMethod:name];
}

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        id p = [[Person alloc] init];
        [p appendString:@""];
    }
    return 0;
}
```


看看class_addMethod函数：

```
BOOL class_addMethod(Class cls, SEL name, IMP imp, const char *types)
参数说明：
cls：添加方法的类
name：selector名称
imp：selector对应得具体实现
types：一个定义该函数返回值类型和参数类型的字符串
```

#### 备援接收者

` - (id) forwardingTargetForSelector:(SEL)selector`

通过此方案我们可以组合来模拟出***多重继承***的某些特性


