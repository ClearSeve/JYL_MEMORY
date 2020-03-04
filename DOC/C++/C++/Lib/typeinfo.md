# typeinfo

```
可以进行运行时类型识别  
typeid(变量或类型)  
可以获得对象的类型信息  
string typeName = typeid(int).name();  

B *pb = new B();      A *pa = pb;

cout<<typeid(*pa).name()<<endl;   
if(typeid(*pa)==typeid(*pb)){cout<<"=="<<endl;}

typeid和dynamic_cast<>()在转换成相应的子类类型时，
有继承关系/需要父类有虚函数
```