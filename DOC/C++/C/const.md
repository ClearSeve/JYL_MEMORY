# const

常用于定义常量，修饰函数的输入参数、返回值。  
void  ab(const  int  *p);  
接收时可扩展接收范围：  
const  int  *p1=const  int  *p2；  
const  int  *p1=int  *p2；  
对于函数void  ab(int  a);  const修饰无接收范围意义，因为函数中，a接收一个值时，a=x是复制一个变量，const  int  a=x时，传入参数时是在做const型的a的初始化。

当一个变量为const型时，可通过指针修改  

1. const  int  a=10;  
   int  *p=&a;  
   *p=20;可修改a的值，编译时有警告，无错误
2. int  a=10;  
   const  int  *p=&a;  
   int  **pp=&p;  
   **pp=20;不可通过p修改变量，可以使用二级指针
3. int  a；  
   const  int  b；  
   int  *p=&a；  
   a，b地址连续，p+1可访问到b的位置  

c++不可修改const变量
