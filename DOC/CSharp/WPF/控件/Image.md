# Image

ActualWidth属性表示显示图像后，控件实际的宽度，五图像时该值为0  

## 加载图像显示

img.Source = new BitmapImage(new Uri(@"D:\1.jpg", UriKind.RelativeOrAbsolute));

## 加载jpg流显示

```
//从文件的byte[]数据数据加载
MemoryStream ms = new MemoryStream(imgDat);
ImageSourceConverter imgSrcCvt = new ImageSourceConverter();
img.Source = imgSrcCvt.ConvertFrom(ms) as System.Windows.Media.Imaging.BitmapFrame;
```

## 转出jpg流

JpegBitmapEncoder encoder = new JpegBitmapEncoder();
encoder.Frames.Add(BitmapFrame.Create((BitmapSource)imgMeasure.Source));
System.IO.MemoryStream ms = new System.IO.MemoryStream();
encoder.Save(ms);
ms.Seek(0, SeekOrigin.Begin);
byte[] img = new byte[ms.Length];
ms.Read(img, 0, (int)img.Length);


## 显示bmp

```
using (MemoryStream stream = new MemoryStream())
{
    m_CurBmp.Save(stream, System.Drawing.Imaging.ImageFormat.Jpeg);
    stream.Position = 0;
    BitmapImage result = new BitmapImage();
    result.BeginInit();
    result.CacheOption = BitmapCacheOption.OnLoad;
    result.StreamSource = stream;
    result.EndInit();
    result.Freeze();
    imgCtl.Source = result;
}
```


## 格式转换
```
ublic System.Windows.Media.Imaging.BitmapSource ConvertBitmap(Bitmap source)
{
   return System.Windows.Interop.Imaging.CreateBitmapSourceFromHBitmap(
                  source.GetHbitmap(),
                  IntPtr.Zero,
                  Int32Rect.Empty,
                  System.Windows.Media.Imaging.BitmapSizeOptions.FromEmptyOptions());
}
public  Bitmap BitmapFromSource(System.Windows.Media.Imaging.BitmapSource bitmapsource)
{
    Bitmap bitmap;
    using (var outStream = new MemoryStream())
    {
        System.Windows.Media.Imaging.BitmapEncoder enc = new System.Windows.Media.Imaging.BmpBitmapEncoder();
        enc.Frames.Add(System.Windows.Media.Imaging.BitmapFrame.Create(bitmapsource));
        enc.Save(outStream);
        bitmap = new Bitmap(outStream);
    }
    return bitmap;
}
```