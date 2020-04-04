# WIN32DLL

## 分配控制台
```
private const string Kernel32_DllName = "kernel32.dll";
[DllImport(Kernel32_DllName)]
private static extern bool AllocConsole();
```

## 隐藏鼠标
```
[DllImport("user32.dll", EntryPoint = "ShowCursor", CharSet = CharSet.Auto)]
//或者CharSet.Unicode
public static extern int ShowCursor(bool bShow);

使用时：
ShowCursor(false);
```

## 读写ini

```
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
```

## 发送消息

```
[DllImport("User32.dll")]
private static extern uint SendMessage(uint hWnd, uint Msg,  uint wParam,  uint lParam);
[DllImport("User32.dll")]
private static extern uint FindWindow(string className, string windowName);

uint  hWnd=FindWindow(null, "name");
if (hWnd >0)
{
    SendMessage(hWnd, 0x0400 + 1, 0, 0);//代表WM_USER
}
```

## 关机

```
[DllImport("user32.dll")]
static extern bool ExitWindowsEx(uint uFlags, uint dwReason);

ExitWindowsEx(0x01,0); //注销:0x00 关机：0x01 重启：0x02
```