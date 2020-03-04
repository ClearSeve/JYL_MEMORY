# ribbon

## 隐藏右键菜单和系统功能

```
class CMFCRibbonBarEx:public CMFCRibbonBar
{
public:
	void RemoveQAT(){ m_QAToolbar.RemoveAll();}
	virtual BOOL OnShowRibbonContextMenu(CWnd* pWnd, int x, int y, CMFCRibbonBaseElement* pHit){return TRUE;}
};
```

##	移除面板

```
m_wndRibbonBar.GetActiveCategory()->RemovePanel(7);

1.12.	自定义窗口
	::CreateWindowEx(0,"button","ok",WS_CHILD|WS_VISIBLE|BS_PUSHBUTTON,400,100,100,40,this->m_hWnd,(HMENU)1001,AfxGetInstanceHandle(),NULL);
创建一个按钮(在有视图时，创建的按钮应放在视图窗口上,且应该写在WM_CREAT消息中，写在Create函数中无效)
或者使用：
CButton *btn=new CButton; 也可以为类内增加一个btn对象成员
	btn->Create("ok",WS_CHILD|WS_VISIBLE|BS_PUSHBUTTON,CRect(0,0,100,40),this,1001);

ON_BN_CLICKED(1001,OnClicked)当按钮被点击时，可对应自定义处理函数OnClicked，其中1001为按钮id，虽然参数为菜单形式，但不是菜单。

	::CreateWindowEx(0,"EDIT","",WS_CHILD|WS_VISIBLE|WS_BORDER,100,100,200,200,this->m_hWnd,(HMENU)1002,AfxGetInstanceHandle(),NULL);
创建一个编辑框
ON_EN_CHANGE(1002,OnEnChange)当编辑框内容发生变化时，可对应自定义处理函数OnEnChange，其中1002为编辑框id。

Edit和button是系统窗口类名，可以不区分大小写
```

## 设置窗口样式

```
在MainFrame类的虚函数PreCreateWindow中可以修改结构体CREATESTRUCT& cs，达到改变窗口样式的目的。
该结构体就是CreateWindowEx函数中12个参数组成的结构。
cs.cx=100;
cs.cy=100;
cs.style | =WS_***; //增加窗口风格
cs.style &=~WS_***;//去掉窗口风格
 
对话框不响应该函数，对话框经过OnInitDialog()

在View类WM_CREATE的OnCreate函数中，可以通过代码，在窗口创建后改变窗口样式。
    SetClassLong(m_hWnd,GCL_HBRBACKGROUND,(LONG)GetStockObject(BLACK_BRUSH));//设置黑色背景
	    SetClassLong(m_hWnd,GCL_HBRBACKGROUND,(LONG)CreateSolidBrush(RGB(255,0,0))); //设置红色背景
    第二个参数可以是GCL_HICON等，通过定时器动态调用该函数，可以做到窗口样式的动画效果。
```

## 标题改变

显示格式：文档名 —窗口名

## 文档名

```
每次打开文件时，文档名变化，是自动进行的
在文档类中OnNewDocument函数中加入：
SetTitle("文档");
这样可以在新建文件时显示新的文字，而不是未命名
```

## 窗口名

资源中修改String Table中的IDR_MAINFRAME的值。第一个就是窗口名（多文档中此资源只有1项），修改即可。

## 修改窗口风格

可以在框架类的PreCreateWindow函数里修改风格

cs.style&=~FWS_ADDTOTITLE;  //去除标题条中的文档名

## 获取其它窗口

+ 对话框句柄获取
```
HWND hwnd=::FindWindow(NULL,"对话框标题");
可以获得对话框的句柄
```

+ 获取父级指针  
GetParent();  在视图类获得框架类的指针

+	获取应用程序实例
```
AfxGetInstanceHandle（）
可以拿到winMain的第一个参数g_hInstance
```

+ 获取视图类指针  
C**View *p=(C**View *)GetActiveView(); 在框架类中获得视图类的指针

## 框架类处理命令消息

```
在框架类中，可以重写虚函数OnCommand(WPARAM wParam, LPARAM lParam),可以使用，用来截获命令消息，：
	int id=LOWORD(wParam);
	if(ID_NEW==id){
..........
	return TRUE;
	}
```

## 显示/隐藏工具栏/状态栏

```
void CMainFrame::OnShowToolBar() 
{
	if (m_wndToolBar.GetStyle() & WS_VISIBLE)
	{
		//隐藏工具栏
		ShowControlBar(&m_wndToolBar, FALSE, FALSE);
	}
	else
	{
		//显示工具栏
		ShowControlBar(&m_wndToolBar, TRUE, FALSE);
	}
}
void CMainFrame::OnUpdateShowToolBar(CCmdUI* pCmdUI) 
{
	if (m_wndToolBar.GetStyle() & WS_VISIBLE)
	{
		pCmdUI->SetCheck(TRUE);
	}
	else
	{
		pCmdUI->SetCheck(FALSE);
	}
｝
```

## 屏蔽拖动标题栏功能

```
WM_NCLBUTTONDOWN

void CMainFrame::OnNcLButtonDown(UINT nHitTest, CPoint point)
{
	if (HTCAPTION == nHitTest) return;  
	CFrameWndEx::OnNcLButtonDown(nHitTest, point);
}
```

## 最大化

单文档只有第一次打开时最大化问题
```
int CRoadProcApp::ExitInstance()
{	
AfxOleTerm(FALSE);
	CleanState();
	return CWinAppEx::ExitInstance();
}


最大化设置
ParseCommandLine(cmdInfo);
m_nCmdShow=SW_SHOWMAXIMIZED;
```

## 去除标题

```
App::InitInstance
::SetWindowText(AfxGetMainWnd()->GetSafeHwnd(),_T(""));


CMainFrame::PreCreateWindow
cs.style &= ~ FWS_ADDTOTITLE;
```
