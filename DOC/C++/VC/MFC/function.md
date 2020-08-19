# function

## 获取窗口对象

CWnd *pWnd = CWnd::FromHandle(hwnd);
CWnd *pWndMain = AfxGetMainWnd();

## 时间

```
CTime time(2001,1,1,13,13,13);
//构造时间2001年1月1日 13：13：13
//也可以使用类的其他构造函数构造时间

CTime time=CTime::GetCurrentTime();

CString str=time.Format("%Y-%m-%d  %H:%M:%S");
//也可以使用类中的其他函数单独获取年月日等，用作文件名时，不能带冒号
int year=time.GetYear();

//可以让两个时间相减，得到间隔时间
CTimeSpan t = ctime2 - ctime1;
__time64_t  x = t.GetTimeSpan(); //得到两个时间的秒差
t.GetMinutes(); //通过该类函数可以获取两个时间之间年、月、小时等直接的时差
```

## 调试

```
TRACE("xx");//可以将CString输出到调试窗口，debug有效
TRACE(_T("\r\n"));
```

## 消息窗口

### 使用

if(IDCANCEL==AfxMessageBox(_T("close"),MB_OKCANCEL))  return;

### 修改标题

```
重写App类的虚函数DoMessageBox

将函数中内容替换为：
LPCTSTR pOldAppName=m_pszAppName;
m_pszAppName=_T("新标题");
int i=CWinApp::DoMessageBox(lpszPrompt, nType, nIDPrompt);
m_pszAppName=pOldAppName;
return i;
```

## 截获消息

```
在虚函数PreTranslateMessage中：
if(pMsg->message==WM_KEYDOWN){
	//屏蔽关闭按键
if (pMsg->wParam == VK_ESCAPE || pMsg->wParam == VK_RETURN) return TRUE;
}
对于其他程序发的消息，只能接收PostMessage发送的。

if (pMsg->message == WM_SYSKEYDOWN && pMsg->wParam == VK_F4)  return TRUE;
```

## 右键菜单

```
动态创建
CMenu menu;
menu.CreatePopupMenu();
menu.AppendMenu(MF_STRING, 消息id,_T("cmd1"));
POINT pt;
GetCursorPos(&pt);	
menu.TrackPopupMenu(TPM_LEFTALIGN, pt.x, pt.y, this);
```


```
菜单资源IDR_MENU1，使用第一列中的菜单内容

CMenu m_menu;
m_menu.LoadMenu(IDR_MENU1);
POINT pt;
GetCursorPos(&pt);	
m_menu.GetSubMenu(0)->TrackPopupMenu(TPM_LEFTALIGN,pt.x,pt.y,this);
```

## 屏蔽f10

if (pMsg->message == WM_SYSKEYDOWN) if (pMsg->wParam == VK_F10) return TRUE;

## 屏蔽帮助

CXXXApp.cpp中去掉：  
ON_COMMAND(ID_HELP, &CWinApp::OnHelp)
