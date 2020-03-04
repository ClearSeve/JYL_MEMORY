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
