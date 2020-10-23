# 绘制到windows窗口

```
class DrawImg
{
public:
    DrawImg()
    {
        m_hbrush = CreateSolidBrush(RGB(0,0,0));
    }
    void Init(CWnd *pWnd)
    {
        pBmpInfo = (BITMAPINFO *)chBmpBuf;
        pBmpInfo->bmiHeader.biSize = sizeof(BITMAPINFOHEADER);
        pBmpInfo->bmiHeader.biPlanes = 1;
        pBmpInfo->bmiHeader.biCompression = BI_RGB;
        for (int i = 0;i<256;i++)
        {
            pBmpInfo->bmiColors[i].rgbBlue = i;
            pBmpInfo->bmiColors[i].rgbGreen = i;
            pBmpInfo->bmiColors[i].rgbRed = i;
            pBmpInfo->bmiColors[i].rgbReserved = i;
        }
        pWnd->GetClientRect(wrect);
        CDC *pdc = pWnd->GetDC();
        hdc = pdc->GetSafeHdc();
    }
    void Draw(IplImage *pImg)
    {
        pBmpInfo->bmiHeader.biWidth = pImg->width;
        pBmpInfo->bmiHeader.biHeight = pImg->height;
        pBmpInfo->bmiHeader.biBitCount = pImg->depth *  pImg->nChannels;    
        int x = 0, y = 0, width = pBmpInfo->bmiHeader.biWidth, height = pBmpInfo->bmiHeader.biHeight;
        if (NULL != pImg->roi) { x = pImg->roi->xOffset; y = pImg->roi->yOffset; width = pImg->roi->width; height = pImg->roi->height; }    

        FillRect(hdc,wrect,m_hbrush);
        int DesX =0;
        int DesY =wrect.Height();
        int DesWidth = wrect.Width();
        int DesHeight = wrect.Height();
        double ratio = pImg->width/(double)pImg->height;
        int WidthRequest = ratio * wrect.Height();
        int HeightRequest = wrect.Width()/ratio;
        if (wrect.Width() < WidthRequest)
        {
            DesHeight = DesWidth / ratio;
            DesY = (wrect.Height() - DesHeight)/2 + DesHeight;
        }
        else if (wrect.Height() < HeightRequest)
        {
            DesWidth = DesHeight*ratio;
            DesX = (wrect.Width() - DesWidth)/2;
        }
        ::SetStretchBltMode(hdc, COLORONCOLOR);
        ::StretchDIBits(hdc,
            DesX, DesY, DesWidth, -DesHeight,
            x, y, width, height,
            pImg->imageData, pBmpInfo,
            DIB_RGB_COLORS, SRCCOPY);
    }
private:
    char           chBmpBuf[2048];
    BITMAPINFO     *pBmpInfo;
    CRect wrect;
    HDC hdc;
    HBRUSH m_hbrush;
};


=========================使用
DrawImg m_DrawImg;
m_DrawImg.Init(this);
m_DrawImg.Draw(pImg);



Mat img;
int depth = img.depth();
if(CV_8U == depth || CV_8S == depth) depth = 8;
else if(CV_16U == depth || CV_16S == depth) depth = 16;
else if(CV_32S == depth || CV_32F == depth) depth = 32;
else if(CV_64F == depth) depth = 64;
```
