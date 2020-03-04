# const

```
const对象只能调用const函数，声明和实现分开时，函数的const修饰在声明和实现都需要写。
1)const  A  a;    const对象
2)void  ab() const{..........}     const函数（类内）
  void  ab(){.................}
以上两个函数构成重载（非const函数优先调用非const函数，如果无非const函数，就用const）
const函数不能修改普通成员变量，如需修改，则在成员前加mutable修饰
exp： mutable  int  m；
      void  ab()const{m=10;}
```
