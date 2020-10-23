# Function

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

