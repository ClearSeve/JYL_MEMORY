# errno

```
<errno.h>
errno为全局变量，存放错误编号，出错则errno的位置改变，不出错则不变化
perror("str")   自动找到错误编号，并打印错误信息，str为额外的提
             示信息，可以不加
strerror()把错误编号转换成错误信息
printf("%m\n",errno); 打印错误信息
```
