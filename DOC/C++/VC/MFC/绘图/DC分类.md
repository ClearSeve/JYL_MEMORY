# DC分类

## CDC类的对象

父类CObject，封装了win32下所有绘图的API函数，还封装了m_hDC(保存了绘图设备句柄)

## CClientDC

封装了在窗口的客户区中绘图的绘图设备(封装了CDC类，在构造时调用了GetDC，析构时调用了ReleseDC)

## CWindowDC  

```
可在标题栏绘图
CWindowDC  dc(AfxGetApp()->m_pMainWnd);
dc.TextOut(0,0,”xxx”); 可以画到框架窗口栏，因为以框架为句柄
     第三个参数可以直接是CString类的对象str
或者用GetParent()
如果使用GetDesktopWindow()则可以在桌面绘图,即在应用程序窗口外部绘图
```

## CPaintDC

```
封装了在WM_PAINT消息中绘图的绘图设备
只能用于WM_PAINT也就是OnPaint函数中（该函数也可以用CClientDC），其他地方不管用
不能写在函数的if这类语句中，要写在函数最外层
```

## CMetaFileDC

是一个文件dc。在调用这个dc时，使用Ellipse等函数相当于给这个dc发绘图命令，让这个dc存储这些命令，使用时可以直接将所有的绘图命令执行出来。

+ wmf

```
1）创建
CMetaFileDC fileDC;
fileDC.Create();
2）使用
不需要调用OnDraw这类的函数进行重绘
fileDC.Ellipse(point.x-10,point.y-10,point.x+10,point.y+10);
CClientDC dc(this);
HMETAFILE hMetaFile=fileDC.Close();
dc.PlayMetaFile(hMetaFile);
fileDC.Create(); //再次创建
fileDC.PlayMetaFile(hMetaFile);//将之前的图形在fileDC上面再画一次，相当于创建fileDC后把之前的绘图命令放进去
DeleteMetaFile(hMetaFile);
3）保存
	HMETAFILE hMetaFile=fileDC.Close();
	CopyMetaFile(hMetaFile,_T("d:\\f.wmf"));
	fileDC.Create();
	DeleteMetaFile(hMetaFile);
4）打开
	HMETAFILE hMetaFile=GetMetaFile(_T("d:\\f.wmf"));
	fileDC.PlayMetaFile(hMetaFile);
	CClientDC dc(this);
	hMetaFile=fileDC.Close();
	dc.PlayMetaFile(hMetaFile);
	fileDC.Create(); 
	fileDC.PlayMetaFile(hMetaFile);
	DeleteMetaFile(hMetaFile);
```

+ emf

```
1）创建
CMetaFileDC metaDC;
RECT rectHimm,rectPixel;
Init中
   HDC hdcRef=GetDC()->m_hDC;
	int iWidthMM=GetDeviceCaps(hdcRef,HORZSIZE);
	int iHeightMM=GetDeviceCaps(hdcRef,VERTSIZE);
	int iWidthPels=GetDeviceCaps(hdcRef,HORZRES);
	int iHeightPels=GetDeviceCaps(hdcRef,VERTRES);
	rectHimm.left=0;
	rectHimm.top=0;
	rectHimm.right=iWidthMM*100;
	rectHimm.bottom=iHeightMM*100;
	rectPixel.left=0;
	rectPixel.top=0;
	rectPixel.right=iWidthPels;
	rectPixel.bottom=iHeightPels;
	metaDC.CreateEnhanced(GetDC(),NULL,&rectHimm,NULL);
2）使用
   metaDC.Ellipse(point.x-10,point.y-10,point.x+10,point.y+10);

	HENHMETAFILE hemf=metaDC.CloseEnhanced();
	PlayEnhMetaFile(GetDC()->m_hDC,hemf,&rectPixel);
	metaDC.CreateEnhanced(GetDC(),NULL,&rectHimm,NULL);
	metaDC.PlayMetaFile(hemf,&rectPixel);
	DeleteEnhMetaFile(hemf);
3）保存 //打开和保存后无法编辑
	HENHMETAFILE hemf=metaDC.CloseEnhanced();
	CopyEnhMetaFile(hemf,_T("d:\\x.emf"));
	DeleteEnhMetaFile(hemf);
4）打开
	HENHMETAFILE hemf=GetEnhMetaFile(_T("d:\\x.emf"));
	UINT size=GetEnhMetaFileHeader(hemf,0,NULL);
	ENHMETAHEADER *emHeader=(ENHMETAHEADER *)malloc(size);
	GetEnhMetaFileHeader(hemf,size,emHeader);
	RECTL rect1=emHeader->rclBounds;
	RECT rect={rect1.left,rect1.top,rect1.right,rect1.bottom};
	PlayEnhMetaFile(GetDC()->m_hDC,hemf,&rect);
	DeleteEnhMetaFile(hemf);
```

## DC的属性

```
dc.GetBkColor()可以获得背景颜色
CSize size=dc.GetTextExtent(str); 可以获得CString类对象str的尺寸
TEXTMETRIC tm;
dc.GetTextMetrics(&tm); 可以获得dc中字体的格式（如字体高度）
```

## GDI绘图对象

```
CPaintDC，CClientDC,CWindowDC不能使用delete函数删除。
自己定义的CDC，需要用DeleteDC删除；定义的pen和brush要用DeleteObject进行删除。

CPen/CBrush/CFont/CBitmap

CGdiObject  父类CObject
封装了m_hObject(保存了和对象绑定在一起的GDI绘图设备句柄)
```

## CPen

```
CClientDC  dc(this);
CPen  pen(PS_SOLID,10,RGB(255,0,0));//画刷类型非PS_SOLID时，画笔宽度1才有效
CPen  *oldpen=dc.SelectObject(&pen);
dc.SelectObject(oldpen);	
pen.DeleteObject();
```

## CBrush

```
CBrush  brush(RGB(255,0,0));
dc.SelectObject(&brush)
或者CBrush  brush(HS_CROSS,RGB(255,0,0));
或者也可以用CBrush  brush(*bitmap)  位图的画刷
CBrush *pbrush=
CBrush::FromHandle((HBRUSH)GetStockObject(NULL_BRUSH));空画刷

设置画刷背景色，阴影画刷时不设置背景色会是白色
dc.SetBkMode(TRANSPARENT);
```

## CFont

```
dc.SetBkMode(TRANSPARENT);可以设置文字背景透明

CFont font;
font.CreatePointFont(120,"方正喵呜体"); //120代表字体大小12号
dc.SelectObject(&font);//画文字时，没有使用pen和brush
COLORREF clr=dc.SetTextColor(RGB(255,0,0));
COLORREF clBk=dc.SetBkColor(RGB(0,0,255));//设置文字背景色
dc.TextOut(0,0,"xxxx");    //用设置的红色绘制字体 
dc.SetTextColor(clr);
dc.SetBkColor(clBk);      

DrawText(str,rect,DT_LEFT)可在指定矩形区域用指定方式绘制文字

设置字体大小
void SetPdcFont(CDC * pDC, int fontHeight)
{
	CFont font;
	LOGFONT lf;
	memset(&lf, 0, sizeof(LOGFONT));
	lf.lfHeight = fontHeight;
	VERIFY(font.CreateFontIndirect(&lf));
	CFont* def_font = pDC->SelectObject(&font);
}
```
