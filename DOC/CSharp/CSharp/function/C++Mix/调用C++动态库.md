# 调用C++动态库

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


## 字符串传入

+ char*字符串传入

```
//c#中的string自动转换为c++中char*
extern "C" __declspec(dllexport) void __stdcall  fun(const char *str)

public static extern void fun(string str);

fun("测试");
```

+ wchar_t*字符串传入
```
extern "C" __declspec(dllexport) void __stdcall show(wchar_t *str)

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

## 字符串读取
```
extern "C" __declspec(dllexport)  void __stdcall  getStr( char* buf,int bufLen)
{
    strcpy_s(buf, bufLen,"吃饭");
}



public static extern  void getStr(StringBuilder buf,int buflen);

StringBuilder temp = new StringBuilder(1024);
getStr(temp, 1024);   
string result = temp.ToString();




public static extern  void getStr(char* buf,int buflen);
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


```
struct INFO
{
	int     aa;   //结构体对齐 8
	double  bb;
	char    cc[100];
    byte    dd[100];
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
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 100)]
    public byte[] dd;
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
