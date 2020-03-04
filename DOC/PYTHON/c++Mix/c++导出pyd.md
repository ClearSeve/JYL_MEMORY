# c++导出pyd

## dll编写

```
#include "Python.h"
static PyObject *ex_foo(PyObject *self, PyObject *Argvs)
{
	int Argv1(0), Argv2(0);
	if (!PyArg_ParseTuple(Argvs, "ii", &Argv1, &Argv2))
	{//ii表示解析出来两个int型参数
		cout << "parse param failed" << endl;
		return NULL;
	}
	cout << Argv1 << "+ " << Argv2 << " = " << Argv1 + Argv2 << endl;

 	Py_INCREF(Py_None);//Py_INCREF增加PyObject对象的引用计数
	        //Py_DECREF减小PyObject对象的引用计数
		   //为了让Python回收废弃的内存、或者防止Python过早地自动回收内存
	//必须用Py_EDCREF和Py_INCREF来控制PyObject的引用计数
	return Py_None;
}


static PyMethodDef ModulesMethods[] =
{
	{ "fun", ex_foo, METH_VARARGS, "fun() doc string" },
	{ NULL, NULL }
	//fun是导出函数名
	//METH_VARARGS，则用Tuple来传递参数   METH_KEYWORDS，则用Dictionary的Key（键值）来传递参数
	//"fun() doc string"  函数的说明字符串。在Python中就是函数的DocString __doc__。
};
static struct PyModuleDef ModuleDesc = 
{    PyModuleDef_HEAD_INIT,      
      "Module", //内置的模块名   模块名.__name__来获取这个字符串
     "This module is created by C++. And it Add two Integer s!", //模块的DocString，也可以用模块名.__doc__获得
     - 1, //-1  模块在全局范围
	 ModulesMethods 
};


PyMODINIT_FUNC PyInit_DllName(void)//生成文件名必须 DllName.pyd
{
    //Py_InitModule("DllName", ModulesMethods);  py2
	return PyModule_Create(&ModuleDesc);
}
```

## 使用

import DllName  
DllName.fun(1,3)
