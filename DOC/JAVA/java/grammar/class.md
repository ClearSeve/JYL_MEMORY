# Class

```
class类型必须使用new来创建对象：B b=new B();
也可以创建临时对象调用函数：new B().fun();

判断是否是某个类
if (obj instanceof ClassName)

this可以指代当前对象
super指代父类：super();super.fun();
super(xxx)  只能在构造函数第一行调用基类的构造
```

## 修饰符

```
[public][abstract][final] class 类名
[extends父类][implement  接口]{}
抽象类abstract不能实例化一个对象，只能被继承。如Number只能产生一个数的子类，比如Integer或者Float。
final类不能有子类。为了提高系统安全性和定义一个完全类。
接口是消息传递的通道，通过接口，消息才能传递到处理方法中进行处理。
```

## 成员变量

```
成员变量描述了类和对象的状态，有时也称为属性、数据、域。
[public][private][protected][package]
[static][final][transient][volatile]   类型   名称
final可以定义常量

protected变量可以被声明它的类和子类以及同一个包中的类访问。如果子类在其他包，子类的对象可以访问，但子类中由父类产生的对象就不能访问。
package在声明时常省略，即没有修饰的变量为package变量。

final变量在程序运行过程中不能被改变。
transient一般在对象序列化上使用。
volatile用来防止编译器对变量进行优化。
```

## 成员方法

```
[public][private][protected][package]
[static][final][abstract][native][synchronized]
返回值类型   方法名(参数表)[throws 异常类型]
```

## 语句块

```
static   
{  //main函数所在类，需要用static语句块，其他不需要
System.out.println("xxxxxx");
}
在类中直接写的语句块，类似于类的构造函数中执行初始化语句，程序运行初始化时执行。
```

## 继承和多态

```
继承使用关键字extends，子类如果和父类有同名函数时，函数访问权限不能低于父类。此时将子类对象赋值给父类对象，父类调用该函数时为子类的函数。
class Base{public void fun(){System.out.println("Base");}}

class Der extends Base{public void fun(){System.out.println("Der");}}

使用时Base b1 = new Dir();
或者Dir d = new Dir();Base b2 = d;
此时调用fun函数，输出为Dir。

将基类改为接口，可以使用接口实现多态
interface Base{public void fun();}
class Der implements Base{public void fun(){System.out.println("Der");}}

instanceof  用于测试一个对象是否是一个指定类的实例
```

## 抽象类

```
不能被实例化的类，可以有静态方法，通过类名直接调用。

abstract class B//包含抽象方法的类必须声明为abstract
{
	abstract void fun();//抽象方法不能有实现，且声明为abstract
}

class C extends B
{
	void fun(){}
}
```

## 匿名对象

```
创建对象时就调用其中的函数，属于临时对象
new A().show();
```

## 创建对象时加入代码

```
创建A对象时，为A类加入成员变量y，重写show方法，在其中可以直接访问外部变量z
int z = 1;
A a = new A(){int y =1; void show(){System.out.println(x+z+y);}};

(new A(){...}).show();  可以加入代码后创建临时变量使用

如果是直接创建接口的对象，则需要该语法实现接口的函数
```

## 内部类

```
在类A内定义的类B，依赖于A的存在，所以B中的代码可以直接访问到A中的成员。
```
