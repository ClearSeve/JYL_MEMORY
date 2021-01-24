# Mat

```
using namespace cv;
Mat img = imread("d:\\1.png",cv::IMREAD_UNCHANGED);
if(!img.data) return ;
imwrite("D:\\111.jpg",img);
namedWindow("window", CV_WINDOW_AUTOSIZE);
imshow("window", img);


vector<int> compression_params;
compression_params.push_back(CV_IMWRITE_JPEG_QUALITY);
compression_params.push_back(95);
imwrite("xx.jpg", mat, compression_params);
```

## 构造

```
cv::Mat mat(height, width, CV_16UC1);//16UC1表示16位深度1通道
mat.data[pos] = val;

cv::Scalar sc = { r,g,b };
cv::Mat mat(height, width, CV_8UC3,sc);
```


## 修改值
```
cv::Mat src;
cv::Mat temp;

src -= temp;
```


## 转换

```
cv::Mat image = cv::cvarrToMat(ipl);
IplImage result=I;
```

## 图像翻转
//上下翻转 
cv::flip(mat,mat,0);

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

## 缩放
```
Mat dst = Mat::zeros(512, 512, src.type());
resize(img, dst, dst.size());
```

## 获取行
```
uchar* data = img.ptr<uchar>(row);
```