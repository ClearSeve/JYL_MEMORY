# 文件

## 选择文件

filename = uigetfile('*.txt');  
[filename, pathname, filterindex] = uigetfile('*.txt');


## 选择文件夹

第一个参数为默认路径  
path = uigetdir('D:\WorkDoc','请选择文件夹');

## 目录操作

```
li = dir('E:\datasource'); %获取目录下所有内容
nameli = {li.name}; %获取列表中所有遍历结果名称
nameli(1:2)=[]; %去掉前面的.目录和..目录


li = dir('E:\datasource\*.txt'); %遍历目录下所有txt文件
```

## 读文件
fi = importdata('D:\1.txt');  


## 写文件
dlmwrite('文件路径',数据,'delimiter', ',','precision', 6,'newline', 'pc');