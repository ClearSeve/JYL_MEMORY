# valarray

```
valarray<double> va;
va.resize(5, 0);
va[0] = 3.14/2;
va[1] = 11;
va[2] = 25;
va[3] = 4;
va[4] = 44;
valarray<double> moveret = va.cshift(2);//将下标2的元素放在头部，前面的元素全部移位到末尾
int max = va.max();
int min = va.min();
int sum = va.sum();
valarray<double> sinRet = sin(va);//求每个元素的sin值
valarray<double> diyCalRet =  va.apply([](double item) ->double{return item * 10; });//对每个元素进行自定义操作
valarray<double> towArrayOptRet = va + diyCalRet;//两个数组每个元素直接相加
```
