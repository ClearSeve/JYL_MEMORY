# CImage

## 绘图及图像转换

```
#include <atlimage.h>

CImage Img;
if (S_OK != Img.Load(L"D:\\1.jpg")) return;
HDC hdc = GetDC()->m_hDC;
::SetStretchBltMode(hdc, COLORONCOLOR);//缩放绘制时必须设置，否则出现失真
Img.Draw(hdc, 0, 0, 100, 100);

int nchannels = img.GetBPP()/8;

Img.Save(L".\\abc.jpg");
```

## 获取DIB

```
CImage Img;    
Img.Attach(hDib);
Img.Draw(GetDC()->m_hDC,0,0,Img.GetWidth(),Img.GetWidth());
可以代理hdib的销毁操作
```