# SDI

## 绘图消息

```
ON_WM_PAINT()
void CMyFrameWnd::OnPaint(){
    CPaintDC dc(this); 
    dc.TextOut(34,234,"ssf");
}
```

## 鼠标消息

```
ON_WM_MOUSEMOVE()
void CMyFrameWnd::OnMouseMove(UINT nKey,CPoint pt){
...................
  InvalidateRect(NULL,TRUE);
}
```

## 键盘消息

```
ON_WM_CHAR
afx_msg void OnChar( UINT nChar, UINT nRepCnt, UINT nFlags ){
      if(97==nChar){
         AfxMessageBox("a");}
}
```

## 最近打开文档

+ 添加  
theApp.AddToRecentFileList(filePathName);
+ 使用/删除  
```
获取点击的最近文档，返回NULL删除该项目
CDocument* C***App::OpenDocumentFile(LPCTSTR lpszFileName)
{
	return NULL;   
	return CWinAppEx::OpenDocumentFile(lpszFileName);
}
```

## 视图类滚动条

```
将基类改为CScrollView（修改其他消息对应代码基类）

OnInitialUpdate中设置滚动条
CSize size(0,0);
SetScrollSizes(MM_TEXT,size);

该代码可以在其他位置调用进行设置
```

## 菜单

```

File，new，Resource Script，进入资源选项卡，右击资源文件夹，insert，可选择加入各种类型的资源。
添加菜单项目时，如使用f&ile则i就变成热键，即使用
alt+i 可以选中该项目。
文件中需包含头文件
#include "resource.h"（不要写在预编译头文件StdAfx.h中，否则会无法修改菜单id）
```

### 设置菜单

```
   1.在CFrameWnd：：Create参数中设置菜单
pFrame->Create(NULL,”name”,WS_OVERLAPPEDWINDOW,CFrameWnd::rectDefault,NULL,MAKEINTRESOURCE(IDR_MENU1));
后面有缺省参数，可以不写，CFrameWnd：：rectDefault代表四个参数。
   2.在框架窗口的ON_WM_CREATE()消息处理中：
     CMenu  menu;
     menu.LoadMenu(IDR_MENU1);
SetMenu(&menu);
menu.Detach();如果menu是局部变量，用这一句可以将句柄与menu对象断开，防止发生问题
```

### 处理菜单项目

```
 ON_COMMAND(ID,OnNew)
       参数为菜单的id及自定义的处理函数
afx_msg void OnNew();
```

### 设置菜单状态

```
在框架类的构造函数中改变变量，取消自动检测更新菜单可用性，这样才可以自由设置菜单项目的可用性
this->m_bAutoMenuEnable=FALSE;

 ON_WM_INITMENUPOPUP()
      菜单被激活，即将显示时（下拉出菜单的项目时)
 afx_msg void OnInitMenuPopup(CMenu *pop,UINT  
 n,BOOL i);
pop->EnableMenuItem(id, 
          MF_DISABLED|MF_GRAYED);
pop->CheckMenuItem（id，MF_CHECKED）
或者使用;
::CheckMenuItem(pop->m_hMenu,id,MF_CHECKED);
MF_BYCOMMAND代表第一个参数使用ID，如果使用另一个参数MF_BYPOSITION则第一个参数使用从0开始的相对位置
	pop->SetDefaultItem(ID);使菜单项目变粗体，但是只能设置一个菜单项目
```

### 设置图像菜单

```
CBitmap bmp1,bmp2;  在Frame类内定义
	bmp1.LoadBitmap(IDB_BITMAP1);
	bmp2.LoadBitmap(IDB_BITMAP2);
	CMenu *pop=GetMenu();
	pop->GetSubMenu(0)->SetMenuItemBitmaps(1,MF_BYPOSITION,&bmp1,&bmp2);
第一个bmp代表MF_UNCHECKED时显示的位图，第二个代表MF_CHECKED时的显示位图

GetSystemMetrics(SM_CXMENUCHECK)可以获得标记菜单位图应该设置的大小
（注意菜单挂在Frame类中，则只能在Frame中的函数里用GetMenu获取菜单的指针）
SetMenu(NULL); 可以去掉菜单
```

### 简易方式设置菜单项目状态

```
在类向导中，可以为菜单项目id添加COMMAND消息处理，还有一个就是UPDATE_COMMAND_UI
这个消息发生在程序发生变化的时候，也就是有程序自动调用，实时根据情况刷新菜单项目
可以为类增加一个变量x，用x的值进行判断，来决定怎么改变菜单项目：
if(x>0){pCmdUI->Enable();//就可以设置是否可用}
else{pCmdUI->Enable(FALSE);}

pCmdUI->m_nID保存了当前菜单项目对应的ID
```

### 右键菜单

+ 使用方式

```
ON_WM_CONTEXTMENU()
    (使用屏幕坐标系，不需要转换,ClientToScreen可以把客户区坐标转换成屏幕坐标)
afx_msg void OnContextMenu(CWnd *pWnd,CPoint pt);
CMenu menu;
menu.LoadMenu(IDR_MENU1);
CMenu   *pPopup=menu.GetSubMenu(0);
      菜单资源，取第一个顶层菜单项目下的子菜单
pPopup->TrackPopupMenu(TPM_LEFTALIGN,pt.x,pt.y,this);
this也用右键菜单处理函数的第一个参数pWnd

或者选择project里的add to project里的components and controls在里面c++文件夹直接选择popupmenu加入
```

+ 常用方式
```
CMenu menu;
menu.CreatePopupMenu();
menu.AppendMenu(MF_STRING,IDM_MENU1,"名称");	
         //需要为IDM_MENU1定义宏以及命令消息处理
CPoint pt;
GetCursorPos(&pt);//pt为当前鼠标坐标点
menu.TrackPopupMenu(TPM_LEFTALIGN,pt.x,pt.y,this);
menu.DestroyMenu();
```

### 动态添加菜单

```
CMenu menu;
menu.CreatePopupMenu(); //创建一个顶层菜单
GetMenu()->InsertMenu(1,MF_BYPOSITION|MF_POPUP,(UINT)menu.m_hMenu,"顶层菜单");  //在菜单第2个位置插入一个顶层菜单
GetMenu()->AppendMenu(MF_POPUP,(UINT)menu.m_hMenu,"顶层菜单");  //为菜单栏添加一个顶层菜单
menu.AppendMenu(MF_STRING,100,"菜单项2"); //添加一个菜单项目，100为项目id
menu.InsertMenu(0,MF_STRING|MF_BYPOSITION,101,"菜单项1"); //在指定位置插入一个菜单项目
menu.Detach();//menu为局部变量时使用

GetParent()->DrawMenuBar();在view类中动态添加菜单后，需要这句重绘菜单栏

DeleteMenu(0,MF_BYPOSITION）
可以删除菜单顶层菜单或者菜单项目
```

## 工具栏

```
1.相关类
  CToolBarCtrl
父类CWnd，封装了对工具栏控件的操作
  CToolBar
父类CControlBar，封装了工具栏和框架窗口之间关系（停靠位置），另外还包括工具栏的创建
2.使用
  1）添加资源
  2）创建工具栏  CToolBar：：Create/CreateEx
  3) 加载工具栏资源   CToolBar：：LoadToolBar
工具栏资源创建时，将某项的id设置为菜单某项目的id，可与其进行绑定，也可以不绑定菜单。和菜单的项目处理方式相同，删除某项时，用鼠标将小图直接拖出即可。
<afxext.h>
CToolBar  toolbar；在CMyFrame类内定义一个对象

ON_WM_CREATE()
afx_msg int OnCreate(LPCREATESTRUCT pcs);
toolbar.CreateEx(this,TBSTYLE_FLAT,
WS_CHILD|WS_VISIBLE|CBRS_ALIGN_TOP|CBRS_GRIPPER|CBRS_TOOLTIPS|CBRS_FLYBY|CBRS_SIZE_DYNAMIC);
this之后的有缺省值，可以不写后面的
CBRS_GRIPPER 可以使工具栏有把手，可移动
CBRS_TOOLTIPS 使鼠标划过时显示提示信息
CBRS_FLYBY   使鼠标划过时在状态栏显示相应提示信息
CBRS_SIZE_DYNAMIC  使工具栏移动后大小可变
CBRS_ALIGN_TOP的作用：
     1）后面可以不使用DockControlBar函数，但使用把手
         移动的风格失效
     2）使用DockControlBar时第二个参数可使用默认值0；

toolbar.LoadToolBar(IDR_TOOLBAR1)；
加载资源后即可显示

工具栏的停靠（船坞化）
  1）工具栏准备停靠的位置
       CToolBar：：EnableDocking
  2)框架窗口允许停靠的位置
       CFrameWnd：：EnableDocking
  3）框架窗口确定停靠的位置
      CFrameWnd：：DockControlBar
     1），2）必须有交集
toolbar.EnableDocking(CBRS_ALIGN_ANY);
EnableDocking(CBRS_ALIGN_ANY);
DockControlBar(&toolbar)；设置开始时停靠的位置（第二个参数，可省略）写完此三句后才可拖拽工具栏。
AFX_IDW_DOCKBAR_TOP  
AFX_IDW_DOCKBAR_BOTTOM   

AFX_IDW_DOCKBAR_LEFT  
AFX_IDW_DOCKBAR_RIGHT

 toolbar.SetWindowText(“xx”);

在工具栏资源中，选中资源，按回车键可进入属性，在底下的提示内可写提示信息(需要加入风格CBRS_TOOLTIPS)
提示信息格式：
aaa\nbbb
aaa会在状态栏显示
bbb会在鼠标滑过工具栏项目时提示

工具栏的显示和隐藏
CFrameWnd：：ShowControlBar（&toolbar，TRUE，FALSE）
第二个参数为是否显示
第三个参数为是否延迟，一般用FALSE

toolbar.IsWindowVisible()
判断是否处于显示状态

Mousemove中如果有invalidateRect操作，会影响工具栏的显示
```

## 状态栏

```
小格为指示器
  CStatusBar  封装了关于状态栏的操作，以及包括创建状 
             态栏
  <afxext.h>
```

+ 创建

```
CStatusBar statusbar;     CMyFrameWnd类内定义
Create消息处理中创建：
statusbar.CreateEx(this);
```

+ 设置指示器
```
    CStatusBar：：SetIndicators
     1)添加字符串资源（如IDS_POS,IDS_TIME）
  多文档中，指示器不能直接用系统定义好的字符串资源，将其删除，自己定义字符串资源
     2)全局数组
           UINT g_hIndicators[]={0,IDS_POS,***};
           数组内第一个元素为0，显示的是工具栏提示信
           息aaa/nbbb中的aaa（会自动加入字符串资源）
          （需为工具栏加入风格CBRS_FLYBY），第二个和第三个
           参数显示的是字符串资源中的内容。
     3）statusbar.SetIndicators(g_hIndicators,
               sizeof(g_hIndicators)/sizeof(UINT));
```

+ 设置指示器宽度和风格

```
statusbar.SetPaneInfo(1,IDS_POS,SBPS_NORMAL,200);
        1代表数组内的第二个元素,第二个参数貌似没啥
        用。第三个参数代表设置的风格，第四个参数为指
        示器宽度。
```

+ 设置/修改指示器的文本内容
```
statusbar.SetPaneText(1,”文字”);
        1,代表第一个位置。0位置用于最左边动态显示工具栏提示。右边开始可以从1开始数指示器位置。

可用于动态设置指示器内容，如mousemove消息中：
CString str;
str.Format(“%d,%d”,pt.x,pt.y);
statusbar.SetPaneText(1,str);
（鼠标移动消息的处理和状态栏类的声明同在类
CMyFrameWnd中）
```

+ 设置状态栏高度  
StatusBar.GetStatusBarCtrl().SetMinHeight( 30 ) ;

+ 指示器位置

```
CRect rect;
m_wndStatusBar.GetItemRect(1,rect); 
可以得到状态栏中第2格的指示器的位置，该位置是以状态栏为基准的位置，rect.top一般都是2。
该函数不能在OnCreate函数中调用，要等到状态栏创建完成后才能调用。
```

+ 对话框中添加指示器

```
需要添加代码：
	CRect rect;
	GetClientRect(&rect);   参数也可以写成rect
	statusbar.MoveWindow(0,rect.bottom-20,rect.right,20);
```

## 加速键

```
资源：id   加速键
 id可对应菜单项目的id
HACCEL   ：：LoadAccelerators
BOOL ：：TranslateAccelerator

CWinApp类的虚函数
virtual BOOL PreTranslateMessage（MSG *pMsg）;
 HACCEL hAccel=::LoadAccelerators(
AfxGetInstanceHandle(),          MAKEINTRESOURCE(IDR_ACCELERATOR1));

::TranslateAccelerator(this->m_pMainWnd->m_hWnd,
                 hAccel,pMsg);
return CWinApp::PreTranslateMessage(pMsg);
第一个参数得到的是CMyFrameWnd类窗口的窗口句柄，因为菜单消息在CMyFrameWnd中处理
```

## 光标

```
光标资源
ON_WM_SETCURSOR()  
鼠标移动过程中并且在没有捕获鼠标消息情况下连续出现的消息
在设置光标资源的消息处理函数中处理(或者在mousemove中):
afx_msg void OnSetCursor(CWnd *pWnd,UINT nHitTest,UINT message)
if (NULL != m_hcursor)
{
	::SetCursor(m_hcursor);
	return TRUE;
}
return CView::OnSetCursor(pWnd, nHitTest, message);



m_hcursor= LoadCursorFromFile(_T("skin\\curFile.cur"));
m_hcursor = LoadCursor( NULL , IDC_CROSS );
```
