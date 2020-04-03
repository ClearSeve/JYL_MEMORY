# Mat

## 显示

```
using namespace cv;
Mat img = imread("d:\\1.png");
if(!img.data) return ;
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
imwrite("D:\\111.jpg",image);
IplImage result=I;
```

## 裁剪

```
cv::Mat dst = img(cv::Rect(x, y, width, height));
```