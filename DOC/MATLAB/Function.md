# 功能

## 输出显示

disp(a)

## 清理

```
clc  
clear   %clear a b   清理a和b两个变量

close all 
```

## 矩阵运算

计算相邻两个原始差值：  
a = [1,5,7,8];  
b = diff(a);  
结果为4 2 1

查找下标位置：  
a = [1,5,7,3,9];  
b = find(a >=4);  
结果为2 3 5
