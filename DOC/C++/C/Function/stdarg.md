# stdarg

```
stdarg.h

int max(int cnt,...){
    int maxValue=0;
	int i=0; //在va_list v和va_end(v)之间不可定义变量，所有变量的定义必须放在前面
	va_list v;  //定义一个变长参数列表
	va_start(v,cnt); //将参数列表定位到第二个参数位置（cnt是对…包含几个参数进行说明】）
	maxValue=va_arg(v,int); //给maxValue赋的值为列表中当前位置的值，同时指针指向下一个值
	for(i=1;i<cnt;i++){	
	int data=va_arg(v,int);
		if(data>maxValue)
			maxValue=data;
	}
	va_end(v); //释放变成参数列表
	return maxValue;
}

调用max()函数时，可用max(2,34,123,23),内部参数数量可以任意
```