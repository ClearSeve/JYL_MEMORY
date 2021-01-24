# BitmapImage


## jpg流转换成BitmapImage

```
BitmapImage bmpim = null;
try
{
    bmpim = new BitmapImage();
    bmpim.BeginInit();
    bmpim.StreamSource = new MemoryStream(imgBytes);
    bmpim.EndInit();
}
catch { bmpim = null; }
```

## bitmapImage转换成jpg流

```
bmpim.StreamSource.Seek(0, SeekOrigin.Begin);
bmpim.StreamSource.Read(imgBytes, 0, imgBytes.Length);
```


## BitmapImage转bmp

```
public System.Drawing.Bitmap BitmapImage2Bitmap(BitmapImage bitmapImage)
{
    using (System.IO.MemoryStream outStream = new System.IO.MemoryStream())
    {
        BitmapEncoder enc = new BmpBitmapEncoder();
        enc.Frames.Add(BitmapFrame.Create(bitmapImage));
        enc.Save(outStream);
        System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap(outStream);

        return bitmap; 
    }
}
```