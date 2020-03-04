# C++Mix

## C++调用C#

c++工程必须clr支持
导出类和函数必须public

```
#using "Csharp.dll"
using namespace AddSpace;
Add^ add = gcnew Add();  
add.fun();
```


捕获c#异常
```
try{ ....}
catch (System::Exception^ e){}
```


托管类不能作为类成员或全局变量
可以通过在类内创建类本身的全局变量进行全局使用
```
public class AA
{
        public static AA obj = new AA();
        
        public void run(string str){.....}
｝
```
c++调用时：
AA::obj->run(str)

## C#调用C++

### 调用CLR类库
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
### 数据类型

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

### 调用动态库

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

### c++回调c#

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

### 字符串传递
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

### 数组传递

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

### 结构体传递

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

### 内存拷贝
 IntPtr datpr = datptr;  
 Marshal.Copy(dat, 0, datpr,dat.Length);  

### 1.1.WIN32DLL
1.1.1.分配控制台
private const string Kernel32_DllName = "kernel32.dll";
[DllImport(Kernel32_DllName)]
private static extern bool AllocConsole();
1.1.2.隐藏鼠标
[DllImport("user32.dll", EntryPoint = "ShowCursor", CharSet = CharSet.Auto)]
//或者CharSet.Unicode
public static extern int ShowCursor(bool bShow);

使用时：
ShowCursor(false);
1.1.3.读写ini
[DllImport("kernel32")]
private static extern long WritePrivateProfileString(string section, string key, string val, string filePath);
[DllImport("kernel32")]
private static extern int GetPrivateProfileString(string section, string key, string def, StringBuilder retVal, int size, string filePath);

写入：
WritePrivateProfileString("section", "key", "xx", ".\\1.ini");

读取：
StringBuilder temp = new StringBuilder(1024);
string result;//接收用的字符串
if (0 == GetPrivateProfileString("section"," key", "", temp, 500, ".\\1.ini"))
{
        result = null;
}
else
{
        result = temp.ToString();
}
1.1.4.发送消息
[DllImport("User32.dll")]
private static extern uint SendMessage(uint hWnd, uint Msg,  uint wParam,  uint lParam);
[DllImport("User32.dll")]
private static extern uint FindWindow(string className, string windowName);

uint  hWnd=FindWindow(null, "name");
if (hWnd >0)
{
    SendMessage(hWnd, 0x0400 + 1, 0, 0);//代表WM_USER
}
1.1.5.关机
[DllImport("user32.dll")]
static extern bool ExitWindowsEx(uint uFlags, uint dwReason);

ExitWindowsEx(0x01,0); //注销:0x00 关机：0x01 重启：0x02
