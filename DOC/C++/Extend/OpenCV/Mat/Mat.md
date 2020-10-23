# Mat

```
using namespace cv;
Mat img = imread("d:\\1.png",cv::IMREAD_UNCHANGED);
if(!img.data) return ;
imwrite("D:\\111.jpg",img);
namedWindow("window", CV_WINDOW_AUTOSIZE);
imshow("window", img);

```

## 构造

```
cv::Mat mat(height, width, CV_16UC1);//16UC1表示16位深度1通道
mat.data[pos] = val;
```

## 转换

```
cv::Mat image = cv::cvarrToMat(ipl);
IplImage result=I;
```

## 颜色转换

```
cv::Mat rgbImg(srcShow.size(),CV_8UC3);
cvtColor(srcShow, rgbImg, cv::COLOR_GRAY2BGR);
```

## 深度转换
mat1.convertTo(mat1, CV_16U);


## 裁剪

```
cv::Mat dst = img(cv::Rect(x, y, width, height));
```

## 获取行
```
uchar* data = img.ptr<uchar>(row);
```