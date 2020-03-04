# c++调用python

文件名一定不能是test，abc 系统模块有。  

不存在#include <inttypes.h>  
屏蔽该行，包含#include <stdint.h>  

没有python36_d.lib  
pyconfig.h中  
pragma comment(lib,"python36_d.lib")
改为python36.lib

## Py文件

文件名MyTestMul  

```
def add(i,j):  
   return i+j
```

## c调用

```
Py_SetPythonHome(L"C:\\py363");
Py_Initialize();
if ( !Py_IsInitialized())
{
   cout<<"Py_Initialize 错误"<<endl;
   return;
}


PyObject* model = PyImport_ImportModule("MyTestMul");
if (NULL == model)
{
   cout<<"PyImport_ImportModule 错误"<<endl;
   return;
}


PyObject* pfun = PyObject_GetAttrString(model,"add");

PyObject* pParm = PyTuple_New(2);
PyTuple_SetItem(pParm, 0, Py_BuildValue("i",3));
PyTuple_SetItem(pParm, 1, Py_BuildValue("i",4));

PyObject* pRetVal = PyEval_CallObject(pfun, pParm);
int retVal = -1;  
int ret = PyArg_Parse(pRetVal, "i", &retVal);
if (0 == ret)
{
cout<<"PyArg_Parse错误"<<endl;
return;
}

//Py_DECREF(pfun);
//Py_DECREF(pParm);
//Py_DECREF(pRetVal);

Py_Finalize();
```

## 参数构造

```
PyObject *param = Py_BuildValue("(i,i,i)", 123, 456, 789);//  构造元祖数据

PyObject * pList = PyList_New(0);
for (int i = 0; i < 3; i++)   PyList_Append(pList, Py_BuildValue("i", i));
```

|字符             |含义        |
|:-:              |:-:        |
|s或者z           |(string) [char *]   将C字符串转换成Python对象，如果C字符串为空，返回NONE |
|s#或者z#         |(string) [char *, int] :将C字符串和它的长度转换成Python对象，如果C字符串为空指针，长度忽略，返回NONE。|
|z                |(string or None) [char *] :作用同s|
|i或b、l（long）   |(integer) [int] :将一个C类型的int转换成Python int对象|
|c                |(string of length 1) [char] ：将C类型的char转换成长度为1的Python字符串对象。|
|d或f             |(float) [double] :将C类型的double转换成python中的浮点型对象。|
|O&               |(object) [converter, anything] ：将任何数据类型通过转换函数转换成Python对象，这些数据作为转换函数的参数被调用并且返回一个新的Python对象，如果发生错误返回NULL。|
|(items)          |(tuple) [matching-items] ：将一系列的C值转换成Python元组。|
|[items]          |(list) [matching-items] ：将一系列的C值转换成Python列表。|
|{items}          |(dictionary) [matching-items] ：将一系类的C值转换成Python的字典，每一对连续的C值将转换成一个键值对。|
