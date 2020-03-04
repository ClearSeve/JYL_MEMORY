# python使用dll

import ctypes

```
字符串传递需要转换格式(c++接收参数为char*)
cstr=str.encode("gbk")

cdecl调用方式：
ctypes.cdll.LoadLibrary
Stdcall调用方式：
ctypes.windll.LoadLibrary
```

## 类型映射

|ctypes数据类型    |     C数据类型 |
|:-:              |:-:          |
|c_char           |     char |
|c_short          |     short |
|c_int            |       int |
|c_long           |     long |
|c_ulong          |    unsign long |
|c_float          |      float |
|c_double         |   double |
|c_void_p         |    void |
对应的指针类型是在后面加上"_p"，如int*是c_int_p  
a = ctypes.c_double(5.3)

## 函数调用

```
extern "C" __declspec(dllexport) char* fun(int *a,char *buf,int bufLen)
{
   *a = *a * 10;
   strcpy_s(buf, bufLen, "xxxx");
   return "abcdef";
}
```

```
#coding=utf-8
import ctypes
#加载dll
dl = ctypes.cdll.LoadLibrary(r'C:\DT.dll')

#创建字符串缓冲区
bufLen = 100
strBuf = ctypes.create_string_buffer('\0',bufLen)

#创建int*  byref将数值转换为指针
a = ctypes.c_int(5)
aRef = ctypes.byref(a)

#执行dll中的函数
pchar = dl.fun(aRef,strBuf,bufLen)

#将dll返回的char*进行转换
szbuffer = ctypes.c_char_p(pchar)


print a.value
print szbuffer.value
print strBuf.value

"""
sBuf = '123456789'
pStr = ctypes.c_char_p( )
pStr.value = sBuf
bufLen = len(pStr.value)
"""
```

## 结构体交互

```
struct  DatStu
{
    int    num;
    char   val[512];
};
extern "C" __declspec(dllexport) void fun(DatStu *stu)
{
    stu->num = 10;
    strcpy_s(stu->val, 512, "xxx");
}




class DatStu(ctypes.Structure):
    _fields_ = [ ("num", ctypes.c_int),
              ("val", ctypes.c_char * 512)]


dl = ctypes.cdll.LoadLibrary(r'C:\DT.dll')

dat = DatStu()
dl.fun(ctypes.byref(dat))

print dat.num
print dat.val
```
