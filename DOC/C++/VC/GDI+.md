# GDI+

## 初始化和释放

```
#include <comdef.h>
#include <GdiPlus.h>
#pragma comment(lib,"gdiplus.lib")
using namespace Gdiplus;

=======旧版本
#ifndef ULONG_PTR
#define ULONG_PTR unsigned long
#endif

初始化：
GdiplusStartupInput gdiplusStartupInput;
ULONG_PTR  pGdiToken;
GdiplusStartup(&pGdiToken,&gdiplusStartupInput,NULL);

释放：
if (pGdiToken>0)   GdiplusShutdown(pGdiToken);
```

## Image

+ GetEncoderClsid

```
bool GetEncoderClsid(const WCHAR* format, CLSID* pClsid)  
{  
    UINT  num = 0;  
    UINT  size = 0;  
    ImageCodecInfo* pImageCodecInfo = NULL;  
    GetImageEncodersSize(&num, &size);  
    if(size == 0)  return false;
    pImageCodecInfo = (ImageCodecInfo*)(malloc(size));  
    if(pImageCodecInfo == NULL)  return false;  
    GetImageEncoders(num, size, pImageCodecInfo);  
    for(UINT index = 0; index < num; ++index)  
    {  
        if( wcscmp(pImageCodecInfo[index].MimeType, format) == 0 )  
        {  
            *pClsid = pImageCodecInfo[index].Clsid;  
            free(pImageCodecInfo);  
            return true;  
        }
    }  
    free(pImageCodecInfo);  
    return false;  
}

用法：
CLSID encoderClsid
GetEncoderClsid(L"image/jpeg", &encoderClsid);
GetEncoderClsid(L"image/png", &encoderClsid);
GetEncoderClsid(L"image/bmp", &encoderClsid);
GetEncoderClsid(L"image/gif", &encoderClsid);
GetEncoderClsid(L"image/tiff", &encoderClsid);
```

+ 绘制图像

```
Image image(L"d:/1.jpg");
Graphics graphics(hdc); 
graphics.DrawImage(&image,0,0,100,100);
```

+ 获取尺寸

```
int width=image.GetWidth();
int height=image.GetHeight();
```

+ 格式转换

```
Image img(L"D:\\b.bmp");
CLSID jpgClsid;
GetEncoderClsid(L"image/jpeg", &jpgClsid);
if( Ok != img.Save(L"D:\\xxx.jpg",&jpgClsid)){..}
```

+ 获取缩略图  

```
Image *pImg = img.GetThumbnailImage(50,50);  
delete pImg;
```

## 颜色

第一个参数数透明度，0代表全透明，255代表不透明  
Color drawClr(AlphaVal,GetRValue(clr),GetGValue(clr),GetBValue(clr));  

## 画笔

Pen( const Color& color, REAL width);  
Pen( const Brush* brush, REAL width);  
pen.SetDashStyle(DashStyleDash);//虚线  

## 画刷

```
//实心画刷
SolidBrush brush(drawClr);

//阴影画刷   bkColor为透明时可以看到效果
HatchBrush brush(HatchStyleHorizontal, forColor, bkColor);  

//渐变画刷
LinearGradientBrush brush(pointA, pointB, Color(AlphaVal,255,0,0),Color(AlphaVal,255,255,0));

//图像画刷
Image image(L"img.jpg");  
TextureBrush tBrush1(&image);  
```

## 绘制直线

```
#define  WIDTH  2

Graphics graphics(hDC);
Pen newPen(drawClr,WIDTH);
graphics.DrawLine(&newPen,10,30,100,100);
```

## 绘制矩形

```
SolidBrush brush(Color::BurlyWood);
GetClientRect(rect);
          graphics.FillRectangle(&brush,rect.left,rect.top,rect.right,rect.bottom);
//FillRectangle绘制实心矩形   DrawRectangle绘制矩形边框
```

## 绘制文字

```
SolidBrush brush(drawClr);
PointF pf(pt.x,pt.y);  
Gdiplus::Font font(_T("宋?体?"), 10);  
Graphics graphics(MemDC.m_hDC); 
graphics.DrawString(name.GetBuffer(),name.GetLength(), &font,pf,&brush);  
```
