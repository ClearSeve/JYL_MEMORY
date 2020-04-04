# C++调用CSharp

c++工程必须clr支持
导出类和函数必须public

```
#using "Csharp.dll"
using namespace AddSpace;
Add^ add = gcnew Add();  
add.fun();
```


捕获c#异常
```
try{ ....}
catch (System::Exception^ e){}
```


托管类不能作为类成员或全局变量
可以通过在类内创建类本身的全局变量进行全局使用
```
public class AA
{
        public static AA obj = new AA();
        
        public void run(string str){.....}
｝
```
c++调用时：
AA::obj->run(str)
