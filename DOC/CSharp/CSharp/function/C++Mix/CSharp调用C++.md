# CSharp调用C++

## 调用CLR类库
建立C++CLR类库，该类库生成的dll，可以直接在c#中添加引用后使用该命名空间内的类。
与C#交互的函数中不能包含c#不支持的类型，否则在C#中不会提示相应函数

字符串传递
```
String^ Fun()
{
     CString ss = L"吃饭睡觉";
     String^ str = gcnew String(ss.GetBuffer());
     return str;
}

void Fun(String^ str)
{
   CString ss(str);
   char* ch2 = (char*)(void*)System::Runtime::InteropServices::Marshal::StringToHGlobalAnsi(str);
}
```
## 数据类型

|  C/C++    | C#     |长度  |
| :-:       | :-:    | :-: |
| short 	|  short |2    |
| int	    |  int	 |4    |
| long	    |  int 	 |4    |
| bool	    |  bool	 |1    |
| char	    |  byte	 |1    |
| wchar_t	|  char	 |2    |
| float	    |  float |4    |
| double	|  double|8    |

## 调用动态库

```
[DllImport("**.dll", EntryPoint = "fun", CallingConvention = CallingConvention.StdCall, CharSet = CharSet.Ansi)]
public static extern void fun();
```

using System.Runtime.InteropServices;  
导入的函数中，字符串类型都用string，数字可以用uint、long等。   
C++指针类型double可以在C#用 ref double或out double 或者double*

句柄可以用uint或者IntPtr   
using System.Windows.Interop;   
IntPtr hwnd = new WindowInteropHelper(this).Handle;


## c++回调c#

```
typedef void (__stdcall *RR)(int x);
extern "C" __declspec(dllexport)  void  fun(RR rr)
```

```
public delegate void RR(int x);
[DllImport("Dll1.dll", EntryPoint = "fun") ]
public static extern void fun(RR rr);

fun((int xx)=> { Console.WriteLine(xx); });
```

## 内存拷贝
IntPtr datpr = datptr;    
Marshal.Copy(dat, 0, datpr,dat.Length);  


## 字符串传递
```
extern "C" __declspec(dllexport) void __stdcall show(wchar_t *str)
```
```
public static extern unsafe void show(char* str);
unsafe
{
    string str = "abc吃饭def";
    fixed (char* p = &(str.ToCharArray()[0]))
    {
        show(p);
    }
}
```


```
extern "C" __declspec(dllexport) void __stdcall getStr(char *outStr)
{
	strcpy_s(outStr, MAX_PATH, "abc吃饭def");
}
```
```
public static extern unsafe void getStr(byte* outStr);
unsafe
{
    byte[] str = new byte[260];
    fixed (byte *p = &str[0])
    {
       getStr(p);
     }
     string ret = Encoding.Default.GetString(str);
     int pos = ret.IndexOf('\0');
     ret = ret.Substring(0, pos);
}
```

## 数组传递

```
extern "C" __declspec(dllexport) void __stdcall showArry(float *pDat,int len)
```

```
public static extern unsafe void showArry(float* pDat, int len)
float[] dat = new float[3] { 1.1f, 2.2f, 3.3f };
fixed (float* parry = &dat[0])
{
     showArry(parry, dat.Length);
}
```

## 结构体传递

```
struct INFO
{
	int    aa;   //结构体对齐 8
	double  bb;
	char  cc[100];
};
void __stdcall testrun(INFO *info)
{
	info->aa = 1;
	info->bb = 2.2;
	strcpy_s(info->cc, "吃饭");
}
```

```
=======调用方式1
[StructLayout(LayoutKind.Sequential)]
public struct INFO
{
       public int aa;
       public double bb;
       [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 100)]
        public string cc;
}
 [DllImport("AA.dll", EntryPoint = "testrun")]
 public static extern void testrun(ref INFO info);


INFO info = new INFO();
testrun(ref info);


=======调用方式2
[DllImport("AA.dll", EntryPoint = "testrun")]
public static extern void testrun(IntPtr pv);


IntPtr pv = Marshal.AllocHGlobal(8+8+100);
testrun(pv);
int aa = Marshal.ReadInt32(pv, 0);
byte[] b = new byte[8];
for(int i = 0; i < 8;++i)
      b[i] = Marshal.ReadByte(pv, 8+i);
double bb = (double)BitConverter.ToDouble(b,0);
string cc =  Marshal.PtrToStringAnsi(pv + 8+8);

Marshal.FreeHGlobal(pv); 
```

## 结构体序列化

```
T ByteToStructure<T>(byte[] dataBuffer)
{//INFO inf = ByteToStructure<INFO>(by);
    object structure = null;
    int size = Marshal.SizeOf(typeof(T));
    IntPtr bufferIntPtr = Marshal.AllocHGlobal(size);
    try
    {
        Marshal.Copy(dataBuffer, 0, bufferIntPtr, size);
        structure = Marshal.PtrToStructure(bufferIntPtr, typeof(T));
    }
    finally
    {
        Marshal.FreeHGlobal(bufferIntPtr);
    }
    return (T)structure;
}

public  byte[] StructureToByte<T>(T structure)
{
    int size = Marshal.SizeOf(typeof(T));
    byte[] buffer = new byte[size];
    IntPtr bufferIntPtr = Marshal.AllocHGlobal(size);
    try
    {
        Marshal.StructureToPtr(structure, bufferIntPtr, true);
        Marshal.Copy(bufferIntPtr, buffer, 0, size);
    }
    finally
    {
        Marshal.FreeHGlobal(bufferIntPtr);
    }
    return buffer;
}
```
