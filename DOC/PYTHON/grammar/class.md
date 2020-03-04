# Class

## 定义

```
_xxx          保护成员  
__xxx        私有成员  
__xxx__     系统定义名字  
dir(类名)   获取类中的属性和方法  


class CC:
   def __init__(self,_name):
       self.name = _name

   def show(self):
       print(self.name)
       print(self.val)


c = CC("abc")
c.show()

类内函数第一个参数都是类的对象  
类的成员变量可以通过self.** 直接创造  
__init__  构造函数  
```

### 继承

在继承类中可以直接覆盖基类的方法

```
class DD(CC):
   def __init__(self,nn):
        CC.__init__(self,nn)
   def Sw(self):
       print(self._name)


d = DD("yy")
d.Sw()
```
