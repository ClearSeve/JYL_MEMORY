# CLR

```
int main(cli::array<System::String ^> ^args)
{
    return 0;
}

接收的c#参数：
System::Collections::Generic::List<double>^ listParam
double% refparam
```

## C++调用CSharp

c++工程必须clr支持
C#导出类和函数必须public

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
可以通过在C#类内创建类本身的全局变量进行全局使用
```
public class AA
{
    public static AA obj = new AA();
    public void run(string str){.....}
｝
```
c++调用时：
AA::obj->run(str)

##  CSharp调用CLR类库
建立C++CLR类库，该类库生成的dll，可以直接在c#中添加引用后使用该命名空间内的类。
与C#交互的函数中不能包含c#不支持的类型，否则在C#中不会提示相应函数

字符串传递
```
String^ Fun()
{
     CString ss = L"吃饭睡觉";
     String^ str = gcnew String(ss.GetBuffer());
     return str;
}

void Fun(String^ str)
{
   CString ss(str);
   char* ch2 = (char*)(void*)System::Runtime::InteropServices::Marshal::StringToHGlobalAnsi(str);
}
```
