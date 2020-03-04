# Py文件交互

## 导入包

from abc import *      
from aaa.def import  *   从aaa包中导入其中def.py内的所有内容    
import math  
import math  as  mt      导入包并重新命名  


## 模块引用

```
在b文件中存在类CC
class CC:
   def fun(self):
      print("cc")

其他文件使用时
====导入文件
import  b
c = b.CC()
c.fun()

====导入文件中的类
from b import CC
c = CC()
c.fun()


======函数调用
在b文件中只存在fun函数时
import b
b.fun()

======执行代码
如果b文件中有执行代码，被其他文件导入，因为导入在文件上面，所以b文件中的代码首先执行
1.21.输入输出
a = input("输入a:")               
i = int(raw_input("input:"))

print(a)
print (a,b,c)
```
