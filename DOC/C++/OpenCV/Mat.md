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

## 合并图像

```
cv::Mat src = cv::imread(left.c_str(), cv::IMREAD_ANYCOLOR | cv::IMREAD_ANYDEPTH);
cv::Mat src2 = cv::imread(right.c_str(), cv::IMREAD_ANYCOLOR | cv::IMREAD_ANYDEPTH); 
if (src.rows != src2.rows) return;
cv::Mat MatrixCom(src.rows, src.cols + src2.cols, src.type());
cv::Mat temp = MatrixCom.colRange(0, src.cols);
src.copyTo(temp);
temp = MatrixCom.colRange(src.cols, src.cols + src2.cols);
src2.copyTo(temp);
cv::imwrite(outpathName.c_str(), MatrixCom);
```