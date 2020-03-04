# OpenCvSharp

OpenCvSharp4  
OpenCvSharp4.runtime.win

## 图像显示

Mat src = new Mat("D:\\1.jpg", ImreadModes.AnyColor);
//Cv2.ImShow("im", src);
//Cv2.WaitKey();
Bitmap bitmap = OpenCvSharp.Extensions.BitmapConverter.ToBitmap(src);//using System.Drawing;
using (MemoryStream stream = new MemoryStream())
{
                bitmap.Save(stream, System.Drawing.Imaging.ImageFormat.Jpeg); //不带透明度
                stream.Position = 0;
                BitmapImage result = new BitmapImage();
                result.BeginInit();
                result.CacheOption = BitmapCacheOption.OnLoad;
                result.StreamSource = stream;
                result.EndInit();
                result.Freeze();
                img.Source = result;
}

## 图像裁剪

dstImg = img[y1,y2,x1,x2]

## bmp转换

System.Drawing.Bitmap bitmap = OpenCvSharp.Extensions.BitmapConverter.ToBitmap(src);

## 获取点阵数据

IntPtr datpr = src.Data;

## 创建图像

Mat.Zeros(row, col, OpenCvSharp.MatType.CV_8UC1);

## 获取设置像素

```
int pix = src.At<byte>(i, j);
ret.SetArray(i, j, r);
```

## 获取图像中最大最小值

double min, max;
src.MinMaxIdx(out min, out max);

## 图像合并

Mat src = new Mat("D:\\a.jpg", ImreadModes.AnyColor | ImreadModes.AnyDepth);
Mat src2 = new Mat("D:\\b.jpg", ImreadModes.AnyColor | ImreadModes.AnyDepth);
if (src.Rows != src2.Rows) return;
Mat MatrixCom = new Mat(src.Rows, src.Cols + src2.Cols, src.Type());
Mat temp = MatrixCom.ColRange(0, src.Cols);
src.CopyTo(temp);
temp = MatrixCom.ColRange(src.Cols, src.Cols + src2.Cols);
src2.CopyTo(temp);
Cv2.ImWrite("D:\\c.jpg", MatrixCom);

## 视频操作

```
VideoCapture _capture = new VideoCapture("D:\\1.mp4");
// VideoCapture _capture = new VideoCapture(0);  读取摄像头
_capture.FrameCount;//视频帧数量
_capture.PosFrames = 10；//定位到第10帧
int FrameWidth = (int)_capture.Get(VideoCaptureProperties.FrameWidth);//获取图像宽度

//读取当前帧图像，并定位到下一帧
Mat image = new Mat();
 _capture.Read(image);
 image.Release();
 ```

## 绘制矩形

OpenCvSharp.Point pt1 = new OpenCvSharp.Point(X1, Y1);
OpenCvSharp.Point pt2 = new OpenCvSharp.Point(X2, Y2);
Scalar clr = new Scalar(255, 0, 0);
int thickness = 2;
Cv2.Rectangle(img, pt1, pt2, clr, thickness);

## 绘制文字

OpenCvSharp.Point pt = new OpenCvSharp.Point(X1, Y1);
Cv2.PutText(img, nameStr, pt, HersheyFonts.HersheyComplex, 1,clr1,1);
