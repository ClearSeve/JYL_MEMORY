# Function

## 获取秒差

double timeSpan = (GetTickCount() - time1 )/GetTickFrequency();


## 腐蚀膨胀

```
cvErode()腐蚀后cvDilate()膨胀，叫作开操作，那些离散点或游丝线、毛刺就被过滤
cvDilate()膨胀后cvErode()腐蚀，叫作闭操作，那些断裂处就被缝合。
函数的输入和输出可以是一个指针

IplConvKernel *ConvKernel = cvCreateStructuringElementEx(3, 3, 1, 1, CV_SHAPE_RECT);
//表示3*3矩形进行操作


cvErode(pImg,pImg, ConvKernel, 1);//腐蚀,  最后一个参数代表腐蚀次数
cvDilate(pImg, pImg, ConvKernel, 4);//膨胀
```

## 查找轮廓

```
cvFindContours
需要二值图像，非0作为1，0保持不变。识别1
0是背景，1是图案
会按照边缘连续性，图案中空心则会识别两个轮廓

cvThreashold对单通道操作，一般是对灰度图转换为2值图
cvThreshold(pImg,pImg, 128, 1, CV_THRESH_BINARY);//将大于128的灰度值（白色）变为1，小于128的变成0
第三个参数代表阈值，第四个代表转换后最大值(0,1)
CV_THRESH_BINARY: value > threshold ? max_value : 0
CV_THRESH_BINARY_INV则相反处理



	CvMemStorage* storage = cvCreateMemStorage(0);
	CvSeq* contour = 0;
	cvFindContours(gray, storage, &contour);//gray为2值图像

	cvZero(gray);

	for (; contour != 0; contour = contour->h_next)
	{
		double contourSize = fabs(cvContourArea(contour));//轮廓面积
		///CvRect rect = cvBoundingRect(contour, 0);//轮廓外接矩形

		if(contourSize > 200) continue;//挑选面积小的

		

		cvDrawContours(pImg, contour,
			cvScalar(255,255,255),cvScalar(255,255,255),
			-1,CV_FILLED);
		//pImg是原始图，gray是二值图
		//CV_FILLED代表完全填充，cvScalar参数可以是1到4个，和图像位数相关
	
	}

    cvReleaseMemStorage(&storage);



int GetCountersNum(CvSeq* contour)
{//获取轮廓数量
	int total = 0;
	CvSeq* contour_bak = contour;
	for (; contour != 0; contour = contour->h_next)  total++;
	return total;
}
```

## gamma增亮

```
void Gamma(int width,int height,BYTE *pImg)
{
	static   const double gamma = 0.45;
	static   const double offset = 0.055;
	static   const double threshold = 0.0031308;
	static   const double maxgray = 255;
	static   const double s = ((1 + offset)*pow(threshold, gamma) - offset) / threshold;

	for (int row=0; row< height; ++row)
	{
		for (int col = 0 ; col < width;++col)
		{
			BYTE *pVal = pImg + row*width + col;
			double gray = (*pVal) / maxgray;

			double SI = 0;
			if (gray <= threshold)
				SI = s*gray*maxgray;
			else
				SI = maxgray*((1 + offset)*pow(gray, gamma) - offset);

			*pVal = (BYTE)SI;
		}
	}
}
```

