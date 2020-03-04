# this指针

```
this是指向当前对象的指针，*this代表当前对象。构造对象时，指向正在构建的对象，成员函数中，指向这个函数的对象
当函数形参与成员变量重名时，可以用this区分：
class  A{
     int b;
   public:
     A(int b){this->b=b;}
};
可使用this返回数据，可作为参数传递
```
