# Attribute

using System.Diagnostics;
添加额外信息

## 内置attribute

函数只在debuge下有效

 ```
[Conditional("DEBUG")]
void fun(){}
```

 ```
[Obsolete("过时的方法",true)]    //第二个参数为编译时是否报错
 void fun(){}
 ```

##	自定义attribute

 ```
[AttributeUsage(AttributeTargets.Class, AllowMultiple = false, Inherited = false)]
//代表该Attribute只能用在class上一行，不能连续两行出现相同Attribute，类在继承时不继承该Attribute
class MyAttrbute : Attribute
{
    public string info;
    public MyAttrbute(string msg)
    {
        info = msg;
    }
    public int vale = 0;
}
 ```

使用时：
 [MyAttrbute("xxxxx", vale = 11)]  
  class Test{}

通过反射获取信息：
```

//判断是否包含Attribute
typeof(Obj).GetCustomAttribute(typeof(MyAttrbute), false)


foreach (var attr in typeof(Test).GetCustomAttribute(true))
{//获取Test类的所有Attribute，找到MyAttrbute进行处理
   MyAttrbute myAt = attr as MyAttrbute;
   if (null != myAt)
   {
       Console.WriteLine(myAt.vale);
   }
}
```


## 遍历类型的成员
```
FieldInfo[] fields = obj.GetType().GetFields();
foreach (var field in fields)
{
    string name = field.Name;
    object val = field.GetValue(obj);//获取值
    field.SetValue(obj, "xxx");//设置值
}
```