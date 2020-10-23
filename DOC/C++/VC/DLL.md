# Dll

## 动态库信息查看

Vs命令提示符查看dll依赖项  
dumpbin   -dependents  d:\**.dll  
-headers        查看dll信息  
-exports        查看导出函数  

## 导出声明

+ 导出函数  
extern "C"  _declspec(dllexport) void __stdcall fun(){}

```C
void  __stdcall Run()

def文件：
EXPORTS
Run

工程属性:
连接器->输入->模块定义文件->  xxx.def

使用：
int  typedef  (__stdcall *Fun)();
```


+ 导出类  
class  _declspec(dllexport) CtestBase{};



## 共享数据段

```
不同进程加载同一dll时候，可以共享相同的数据。
#pragma data_seg ("shareData")//shareData为自定义的段名
HWND g_hWnd=NULL;//共享数据必须初始化，否则会变成bss段的
#pragma data_seg()
#pragma comment(linker,"/section:shareData,rws")
```


## 动态库调用

+ 隐式调用
```
需要lib文件，dll文件

声明函数
void show();
库文件使用声明
#pragma comment(lib,"kk.lib")

直接调用
show()
```

+ 显示调用

```
只需要dll文件

typedef void ( __stdcall *pFun)();

HINSTANCE hInstanceLibrary=::LoadLibrary("kk.dll");

pFun fun=(pFun)GetProcAddress(hInstanceLibrary,"show");

fun();

::FreeLibrary(hInstanceLibrary);
```

## 调用类型

+ _stdcall  
Pascal程序的缺省调用方式，参数采用从右到左的压栈方式，被调函数自身在返回前清空堆栈。
WIN32 Api都采用_stdcall调用方式
+ _cdecl  
C/C++的缺省调用方式，参数采用从右到左的压栈方式，传送参数的内存栈由调用者维护。_cedcl约定的函数只能被C/C++调用，每一个调用它的函数都包含清空堆栈的代码，所以产生的可执行文件大小会比调用_stdcall函数的大。
由于_cdecl调用方式的参数内存栈由调用者维护，所以变长参数的函数能使用这种调用约定
+ _fastcall  
调用较快，它通过CPU内部寄存器传递参数。
按C编译方式，_fastcall调用约定在输出函数名前面加“@”符号，后面加“@”符号和参数的字节数，形如@functionname@number。

## 错误原因

```
头文件的作用是声明函数或者变量，如果缺少头文件，会出现找不到标识符的错误。
Lib文件的作用是指明头文件中声明的函数存在，lib文件包含实际代码的进入信息。如果没有引用lib，会出现unresolved external symbol（无法解析的外部符号）的错误。如果引用了lib，但是lib不在系统目录或者当前目录，会出现缺少lib的错误。(lib文件可以在连接器的附加依赖项中设置，也可以在代码中显式声明)
Dll文件存储着函数的实现代码，如果缺少dll，程序运行时会出错。
stl类型的参数无法作为引用或者指针进行传递。
```

## 调试

+ Dll工程调试  
配置属性-》调试-》命令    输入调用exe所在位置

+ EXE调试  
dll和pdb文件需要在一起  
将dll的cpp文件直接在exe中打开即可  

## 窗口交互

+ 创建模式对话框  
函数前添加宏  
AFX_MANAGE_STATE(AfxGetStaticModuleState());

+ 获取外部窗口  
在库文件中需要调用外部窗口对象时，可以使用  
HWND hWnd=GetForegroundWindow();