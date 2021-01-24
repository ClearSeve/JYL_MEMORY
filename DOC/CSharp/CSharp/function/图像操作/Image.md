# Image

```
System.Drawing.Image image = System.Drawing.Image.FromFile(imgPathName);  
image = Image.FromStream(Stream);
```

## 格式转换
```
Bitmap bmp;
using (System.Drawing.Image img = System.Drawing.Image.FromFile(imgPathNameOrg))
{
     bmp = new Bitmap(img.Width, img.Height, System.Drawing.Imaging.PixelFormat.Format32bppRgb);
     using (Graphics g = Graphics.FromImage(bmp))
     {
         g.DrawImage(img, 0, 0);
     }
}
```

## 索引图像判断
```
bool IsIndexedPixelFormat(System.Drawing.Imaging.PixelFormat imagePixelFormat)
{
    System.Drawing.Imaging.PixelFormat[] pixelFormatArray = {
                                    System.Drawing.Imaging.PixelFormat.Format1bppIndexed
                                    ,System.Drawing.Imaging.PixelFormat.Format4bppIndexed
                                    ,System.Drawing.Imaging.PixelFormat.Format8bppIndexed
                                    ,System.Drawing.Imaging.PixelFormat.Undefined
                                    ,System.Drawing.Imaging.PixelFormat.DontCare
                                    ,System.Drawing.Imaging.PixelFormat.Format16bppArgb1555
                                    ,System.Drawing.Imaging.PixelFormat.Format16bppGrayScale
                                };
    foreach (System.Drawing.Imaging.PixelFormat pf in pixelFormatArray)
    {
        if (imagePixelFormat == pf)
        {
            return true;
        }
    }
    return false;
}
```