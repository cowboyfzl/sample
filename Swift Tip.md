SWift相关的东西
1:取min和max
.min取当前最小值，.ma取当前最大值
0b…表示二进制
0o...表示八进制
0x…表示十六进制 
与OC不同：1：超过当前类型的最小值与最大值时，会直接报错，不会循环
                 2：整数与浮点数运算时，需要把整数转换成浮点数，不然会报错
                 3：数值之间可以用 _ 分割一边识别位数

2:空字符串表示方式
var str = “”
var str = string()
字符串拼接直接用+号
字符串转换大小写:.uppercaseString 与 .lowercaseString
与OC不同: 1:字符串对比OC是is EqualXXX而swift是 ==
               2:\()输出变量或常量名

3:选项类型
在类型名称后面加?(选项类型)表示此变量或常量不一定有值，比如 var str:String? 它的初始值就是nil ，!表示一定有值，如果把某种类型设置成!(隐式可选项类型)而不赋值，使用的时候就会报错
与OC不同:1:OC的nil表示它指向不存在的指针，而swift不是指针，它表示一类型无值

4:运算符
！！SWIFT3.0可能会取消++和- -  改为+=1 和-=1

5:循环语句 for in
1.1:1..X, 1..<x
1.2:也可用于输出每个字符 :for c in “哈哈哈”
1.3:如果循环主体不用区间值用_代替 for _ in 1…X
1.4:for循环快速遍历数组下标和对象 for (index,value) in arr.enumerate{}
1.5:for循环为 for i in 1…X 如果要反过来 for i in 1…X..reverse()
！！for循环会取消C语言的标准模式 改为for in


6:switch语句
1.1:switch…case 值可以有多个:case:”a”,”b”,”c”: 用,隔开
1.2:可以使用区间值case:1...10: case:11…20
1.3:可以使用元祖作为参数case(0,0) case(_,0)  _表示任意值可以用绑定值代替:let x
1.4:case后面可以加上where 使条件更严格：case let(x,y) where x==y 同时满足两个条件才会执行
1.5 case 后加fall through表示强制向下执行下一行
与OC不同 不用在每个case后面加break

7:集合类型
1.1:数组表示方法有三种1: var a = [“”,””,””]
                           2: var a:[元素类型比如String] = [value1,value2,value3]
                           3: var a:Array<元素类型比如String> =  [value1,value2,value3]
1.2:创建一个没有元素的数组var arr = [type]() (创建nil数组为?)
1.3:可以通过[type](count:个数, repeatedValue:参数)来加入多个相同的元素
1.4:可以通过+号把两个数组合并
1.5:可以通过sort()排序(非Anyobject类型)
1.6字典的表示方法1:var dic = [key1:value1,key2:value2]
                         2:var dic:Dictionary<keyType,valueType> = [key1:value1,key2:value2] 
	                  3:var dic:[keyType,valueType] = [key1:value1,key2:value2] 
1.7清空字典把[:]赋值给字典,更新字典参数用.updataValue(forKey:)
附加：判断语句如果不能直接比较 就把它的返回值作为常量来比较
if let 常量名 =  .updataValue(forKey:)(此方法会返回旧值)


8:3元组:tuple 
1.1:元祖可以保存仁和类型的元素let k = (“hello,2”) 

9:函数
1.1 函数的返回值可以是多个返回的是一个元组
1.2 函数可以添加外部参数名比如func (from value:Int)
1.3 函数参数可以设置默认值
1.4 函数参数可以是可变参数比如func (value:Int…)
						 func(1,2,3,4,5,6)
1.5 函数参数可以加inout 来改变外部变量
1.6 函数类型:(参数类型，参数类型) ->（返回类型）函数类型也可以作为变量的类型，并且把函数名传入变量即var m:(int,int) - double = 函数名
1.7 函数类型可以作为函数的参数或者函数的返回值   
1.8 嵌套函数返回的也是函数的类型
1.9 如果有相同命名的变量，优先调用局部变量，其次是全局变量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 


10:闭包
1.1 {(paramtetrs)->  return type in statements}
1.2 推导类型{a,b in return a<b} 也可以把return去掉 也可以用速记自变量名$0和$1 还有运算符函数<
1.3 获取值:当一个嵌套函数调用外部变量，会保存这个值，因为对于这个函数而言外部变量就是它的全局变量


11:类与结构体
1.1 两者不同之处:类具有继承功能，可以进行类型转换，可以在运行期检查类的实例类型，释放不必要的空间， 处理引用计数                                                                                                                                                                                                                                                                                                                                                                   
1.2 结构体属于值类型，类属于引用类型
●值类型(Value Types)：每个实例都保留了一分独有的数据拷贝，一般以结构体 （struct）、枚举（enum） 或者元组（tuple）的形式出现
●引用类型(Reference Type)：每个实例共享同一份数据来源，一般以类（class）的形式出现
用=== 或者 !==判断两个引用类型
1.3枚举语法: enum 名称 （:(类型) Raw链接类型）{
case 名称
case名称
也可以写成 case 名称,名称
也可以关联类型 名称(String,int,double)
也可以设置Raw值，case 名称 = 1, 名称 = 2 使用方法枚举名.枚举成员名.rawValue
}

在switch里用 enum  switch state {
case .枚举成员名
case .枚举成员名
}

12:属性方法
1.1:存储属性———分为值类型和引用类型，其区别是前者是指向不同的内存空间，后者指向相同的内存空间
1.2:计算属性 
set(新值名){
或者newValue(速记声明)
} 

get{
return 返回值
}
1.3:只读计算属性只有get没有set也可以吧get去掉直接return
1.4:属性观察者
willSet(新值名){
newValue(速记声明)
}

didSet{
可以获取上次的值oldValue 声明一个变量记录这次的新值，两者比较得出区别
}

1.5:类型属性(类型方法)
在结构体里用 static 标识变量，为共享的意思，不属于任何一个对象所有
使用方法:用当前结构体或枚举名来调用此公共变量(类似于OC的类方法)不需要创建对象
而类的类型属性则需要在属性前加class关键字
类型的属性和方法只能被类型方法所使用，否则会报错
self 代表类本身
1.6.修改值类型的实例方法
如果想要修改值类型的变量(结构体，枚举都属于值类型，上文中已经提到过)必须要在函数前加上mutationg

13:继承
1.1子类可以继承父类的方法和属性，也可以重写重写父类方法时必须在重写的方法前加上 override ！
如果在重写方法后需要调用父类的方法必须用super.相应父类方法
重写父类属性:和重写父类方法一样需要在重写的属性前加上override ，然后调用get{return super.父类属性名 
 }

set{
super.父类属性名 = newValue
}

重写观察者属性
override 属性名 :类型{
didSet{

}

}
如果属性不想被子类重写或修改，应该在属性前加上final 


14:初始化与析构
1.1初始化:init(可以加上需要初始化的变量)
1.2制定构造器与便捷构造器
一:指定构造器必须直接调用其父类的制定构造器
二:便捷构造器必须在同一类中调用另一构造器
三:便捷构造器必须调用制定构造器来结束
制定构造器: init(param){
statements
}

convenience init(param){
…
(super)self.init(param)
}

1.3析构 == deist{ }一个类在销毁的时候会调用 


15:引用计数
解决循环引用机制:弱引用(weak reference)及无助引用:(unowned reference)


16:可选链
1.1:在想要调用属性或方法或索引时，在可选项后面加上问号(?)来制定可选链，看看选项是否为nil
1.2:在调用属性时可以通过判断if let a = 实例化常量.类名?.子类变量来判断是否有值，如果未初始化就是nil,如果把子类实例化时，把它的子类赋值后就有了相应的值，如果赋值了相应的值，就可以用!来确定一定有值
1.3:可选链方法同上

17:类型转换与扩展
1.1:is 可用于判断某个实例是否为某个类的子类
1.2:向下转型谋类的常量或变量可能会实际引用到子类的实例。若是，则可以利用as 类型转型运算符向下转型，因为向下转型可能会失败，所以转换运算符有两个类型as? 可能返回nil 另一种是as!强制转换
1.3:AnyObject用来表示任何类型的实例，Any用于任何类型除函数类型外
1.4:扩展extension 
 扩展属性
extension 需要扩展的类型 (例如Double) {
 var mile:Double {return self*1.6}
 var km:Double {return self }
 var m: Double{ return self/1000}
}
还可以扩展构造函数与方法同上

还可以扩展索引subscript 
extension Int {
subscript (index:Int)->Int {
var base = 1
for var i=1; i<=index i+=1{
base *=10
}
return (self/base)%10
}

print(123456789[1]) 结果是8

18:协议 
1.1: 属性协议
protocol 协议名 {
 var name: String {get set}
}
如果只有get 那只能返回某个参数

1.2：方法协议
protocol 协议名 {
 方法 
}
方法协议 如要改变参数值必须在函数前加上 mutating
如果是结构体继承协议，必须在实现主体内容前也加上mutating  因结构体枚举属于值类型
如果是类在实现协议主体内容前可以省略mutating ,因为类是引用类型

1.3：类型的协议

1.4：协议扩展方式加入

1.5: 协议的继承

1.6：协议的组合<协议1，协议2>

1.7检查是否遵守协议
实例 is 协议名
if let obj = object as? 协议名
@objc 用来表示协议是可选的，也可以用来表示暴露给OC的代码，此外，@objc只对类有效 

19:泛型
1.1:现将不一样的地方画出，然后以T表示，并在函数后面加T
1.2:类型约束,在处理泛型时，有时需要加入一些条件才能处理,先找出相异处，再以T取代比如:<T:Equatable>(参数:T,array:[T]) ->T?
以及:<T:Comparable>(参数:arr[T])
1.3关联类型 typealias ItemType 



tip:swift项目名就是个命名空间，所以不用import
tip:使用KVC要给基本类型初始化赋值，不能设置成可选类型。不能给私有属性赋值，不能给没有初始化的属性赋值
tip:使用KVC给没有的属性赋值要使用func setValue(… forUndefineKey)方法，并且不要实现父类方法就不会崩溃
tip:swift使用运行时后需要free获取的各种列表，不然会内存泄漏
tip:运行时获取不到可选的基本类型，必须初始化，获取不到私有属性（可获取ivar）
tip:如果遇到可以选类型最好加上判断或者guard，for循环里使用guard let 配合contiune 便利下一个
tip:修改plist 可以右键 SourceCode 
tip:便利构造函数可能返回nil,便利构造函数作用是判断给你参数是否符合条件，以节省内存开销，给属性赋值要在self.init()之后，便利构造函数不能被重写或super
tip:构造方法或者函数参数有默认值就可以任意组合
tip:便利构造函数有点像OC里的工厂模式
