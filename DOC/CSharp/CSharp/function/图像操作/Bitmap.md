# Bitmap

## 创建

FileStream fs = new FileStream("D:\\1.jpg", FileMode.Open);  
Bitmap bmp = new Bitmap(fs);//new Bitmap("D:\\1.jpg")  

## 保存

bmp.Save(imgPathName, System.Drawing.Imaging.ImageFormat.Jpeg);//默认存储bmp格式，可选择输出格式


## 画图
```
System.Drawing.Graphics grap = System.Drawing.Graphics.FromImage(bmp);
grap.PageUnit = System.Drawing.GraphicsUnit.Pixel;

int fontSize = 100;
var font = new System.Drawing.Font("宋体", fontSize, System.Drawing.GraphicsUnit.Pixel);
var brush = new System.Drawing.SolidBrush(System.Drawing.Color.Yellow);
grap.DrawString(name, font, brush, txtX, txtY);


int penWidth = 10;
grap.DrawRectangle(new System.Drawing.Pen(System.Drawing.Color.Red, penWidth), new System.Drawing.Rectangle(ptAX, ptAY, ptBX, ptBY ));

grap.Dispose();
```


## 获取图像宽高

```
using (FileStream fs = new FileStream("D:\\1.tif", FileMode.Open, FileAccess.Read))
{
      System.Drawing.Image image = System.Drawing.Image.FromStream(fs);
      int  width = image.Width;
      int  height = image.Height;
}
```

## 修改图像质量

```
public byte[] ChangeImageQuality(byte[] img)
{
    MemoryStream ms = new MemoryStream(img);
    byte[] buffer = null;
    Bitmap bmp = null;
    ImageCodecInfo ici = null;
    System.Drawing.Imaging.Encoder ecd = null;
    EncoderParameter ept = null;
    EncoderParameters eptS = null;
    try
    {
        bmp = new Bitmap(ms);
        ici = this.getImageCoderInfo("image/jpeg");
        ecd = System.Drawing.Imaging.Encoder.Quality;
        eptS = new EncoderParameters(1);
        ept = new EncoderParameter(ecd, 50L);
        eptS.Param[0] = ept;
        //bmp.Save("D:\\xxxx.jpg", ici, eptS);
        MemoryStream stream = new MemoryStream();
        bmp.Save(stream, ici, eptS);
        buffer = new byte[stream.Length];
        stream.Seek(0, SeekOrigin.Begin);
        int len = stream.Read(buffer, 0, buffer.Length);
        if (len != buffer.Length) buffer = null;
    }
    catch (Exception ex)
    {
        return buffer;
    }
    finally
    {
        bmp.Dispose();
        ept.Dispose();
        eptS.Dispose();
    }
    return buffer;   
}
private ImageCodecInfo getImageCoderInfo(string coderType)
{
    ImageCodecInfo[] iciS = ImageCodecInfo.GetImageEncoders();
    ImageCodecInfo retIci = null;
    foreach (ImageCodecInfo ici in iciS)
    {
        if (ici.MimeType.Equals(coderType))
            retIci = ici;
    }
    return retIci;
}
```
