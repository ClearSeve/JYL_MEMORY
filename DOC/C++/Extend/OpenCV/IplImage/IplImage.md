# IplImage

## IplImage构造

```
int depth = IPL_DEPTH_8U;
int channels = 1; //黑白 1，RGB 3
IplImage *pImg = cvCreateImage(cvSize(Width,Height),depth,channels);

for (int row = Height -1 ; row >= 0; --row)
{
	for (int col = 0; col < Width; ++col)
	{
	int pos = pImg->widthStep * row + col;//如果depth为IPL_DEPTH_16U  int pos = pImg->widthStep * row + col*2;   
	pImg->imageData[pos] = dat;
	}
				
}  
```

## IplImage结构体赋值

```
void InitIpl(IplImage &img,char* DibBuf,int width,int height,int depth,int nchannel)
{
		img.nSize = sizeof(IplImage);
		img.ID = 0;                   
		img.alphaChannel =0;     
		img.depth = depth;      
		strcpy_s(img.colorModel,4,"RGB"); 
		strcpy_s(img.channelSeq,4,"BGR");   
		img.dataOrder = 0;     
		img.origin = 0;          
		img.align = 4;                   
		img.roi = NULL;   
		img.maskROI = NULL;      
		img.imageId = NULL;     
		img.tileInfo = NULL;            
		memset(img.BorderMode,4,0);
		memset(img.BorderConst,4,0);   
		img.imageDataOrigin = 0; 
		img.width = width;             
		img.height = height;  
		img.nChannels = nchannel;    
		img.imageSize = img.width * img.height  * img.nChannels;  
		img.widthStep = img.width  * img.nChannels;    
		img.imageData = DibBuf;
}
```

## 读取图像

```
IplImage *pImg = cvLoadImage("D:\\1.png",CV_LOAD_IMAGE_ANYCOLOR|CV_LOAD_IMAGE_ANYDEPTH);	
第二个参数可以不写，默认是将图像以8位3通道载入。
CV_LOAD_IMAGE_GRAYSCALE代表以单通道载入

第二个参数iscolor 
>0  强制以彩色（3通道）
=0  灰度
<0  取决于图像本身
```

## 弹窗显示

```
可以在app类initInstance创建对话框前调用函数，取代对话框。

IplImage* img=cvLoadImage("d:\\1.jpg"); //将图像加载到内存中


cvNamedWindow("picWindow",CV_WINDOW_AUTOSIZE);
//窗口名为picWindow,第二个参数默认0，表示任意拉伸。CV_WINDOW_AUTOSIZE表示窗口大小是图像的大小，不能拉伸

cvShowImage("picWindow",img);
//使用创建的窗口显示图片


cvWaitKey(0);//使程序暂停，等待用户按下键盘，参数0代表一直等待，如果参数为正数x，代表等待x毫秒
cvReleaseImage(&img);//释放内存中的图像
cvDestroyWindow("picWindow");//销毁创建的窗口
```

## ROI

```
cvSetImageROI(img,cvRect(x,y,width,height));
cvResetImageROI(img);
在 cvSetImageROI和cvCopy函数时，一定要注意图像的大小（或ROI大小），避免越界和图像大小不一。
```

## 保存图片

```
可以将img保存为任意格式图片，保存图片的目录必须存在
cvSaveImage("d:\\x.png",img);
```

## 创建与销毁

```
IplImage* img = cvCreateImage(cvSize(width,height),IPL_DEPTH_8U,3);
cvReleaseImage(&img);

创建时，也可以使用cvGetSize(img)作为参数
IplImage* img = cvCreateImage(cvGetSize(src),src->depth,src->nChannels);
```

## 归零

cvZero(img);

## 设置颜色

cvSet(pImg, cvScalar(0x00,0xDB, 0x00));

## 翻转

```
cvFlip(img,img,0);

IplImage中的origin成员为0，代表上下翻转，1代表左右翻转。 
```

## 复制

cvCopy(src, dst);//将src拷贝到dst中  

IplImage*  dst=cvCloneImage(img);//将img完整复制一份

## 获取尺寸

```
int cx=cvGetSize(img).width;  
int cy=cvGetSize(img).height;

或者
int cx=img->width;
int cy=img->height;
```

## 调整大小

```
将src变为dst的大小，dst为缩放后的图像。

IplImage* dst = cvCreateImage(cvSize(200,60),src->depth,src->nChannels);
cvResize(src,dst);

cvReleaseImage(&dst);//放在函数最后（处理图像结束后）释放内存
```

## 裁剪

```
将src进行裁剪，裁剪后的图像为dst。

int x=0,y=0,width=200,height=200;
IplImage* dst = cvCreateImage(cvSize(width,height),src->depth,src->nChannels);
cvSetImageROI(src,cvRect(x,y,width,height));//设置感兴趣的区域，也就是用来操作的区域
cvCopy(src,dst);
```

## 分离通道

```
cvSplit(src,r,g,b,NULL);
将src的rgb分别放到三个通道图中,r,g,b三个IplImage都是单通道的
```

## 平滑处理

```
将src平滑处理后放到dst
cvSmooth(src,dst,CV_GAUSSIAN ,3,3);
第三个以后的参数都有默认值。
第三个参数可以是:
CV_BLUR_NO_SCALE (简单不带尺度变换的模糊) - 对每个象素的 param1×param2 领域求和。如果邻域大小是变化的，可以事先利用函数 cvIntegral 计算积分图像。
CV_BLUR 对每个象素param1×param2邻域 求和并做尺度变换 1/(param1.param2).
CV_GAUSSIAN对图像进行核大小为 param1×param2 的高斯卷积
CV_MEDIAN对图像进行核大小为param1×param1 的中值滤波(邻域是方的).
CV_BILATERAL (双向滤波) - 应用双向 3x3 滤波，彩色sigma=param1，空间 sigma=param2. 平滑操作的第一个参数.
```

## 融合

```
两个图象可以是任何象素类型，只要它们的类型相同。它们可以是单通道或是三通道，只要它们相符。必须有相同的象素类型。这些图象可以是不同的尺寸，但它们的ROI必须有相同的大小。
融合时候，以图像本身大小为基准，而不是以显示界面的大小为基准
IplImage *dst, *src; 
dst存储合成后的图


 int width =200,height =300; //设置ROI大小相同
 cvSetImageROI(dst, cvRect(0,0,width,height)); 
 cvSetImageROI(src, cvRect(0,0,width,height));
//前两个参数代表图像左上角位置
 
 double alpha =0.5; 
 double beta  =1;//设置为0和1可以让上面的图盖在底图上 
 double gamma=0;
 cvAddWeighted(dst, alpha, src,beta,gamma,dst); 
//运算是dst=dst*alpha+src*beta+gamma;
 
 cvResetImageROI(dst); //因为ROI变小，所以重置回原来的大小
```

## 去白色融合

```
去掉src2的白色，融合图放在src3
IplImage* src3 = cvCreateImage(cvSize(width,height),src1->depth,src1->nChannels);
cvAnd(src1,src2,src3);

cvAndS(src,cvScalar(255,255,0),src3); //可以用指定颜色进行位与
```

## 调色

为图片增加红色（红通道加了100，如果是负数代表减少颜色），cvScalar可以有四个参数BGRA  

cvAddS(img,cvScalar(0,0,100),img);

## 反色

```
for(int i=0;i != height; ++ i)   
{  
	for(int j=0;j != width; ++ j)  
	{  
		for(int k=0;k != channels; ++ k)  
		{  
			data[i*step+j*channels+k]=255-data[i*step+j*channels+k];   
		}  
	}  
} 
```

## 边缘检测

```
IplImage* img=cvLoadImage("d:\\1.jpg",CV_LOAD_IMAGE_GRAYSCALE); //灰度图像
IplImage* CannyImg=cvCreateImage(cvGetSize(img),IPL_DEPTH_8U,1);
cvCanny(img,CannyImg,50,150,3);//输出为CannyImg
```

## 遍历IplImage处理图像

```
将img中蓝色最大化

IplImage* img = cvCreateImage(cvSize(src->width,src->height),IPL_DEPTH_8U,3);
cvCopy(src, img);
for(int y=0;y<img->height;y++)
{
	uchar* ptr=(uchar*)(img->imageData+y*img->widthStep);//ptr指向一行的数据长度，也就是指向的内存大小为一行的内存大小
	for(int x=0;x<img->width;x++)
	{
	ptr[3*x+0]=255;//0，1，2分别代表蓝绿红，3*x是因为这是3通道的图像
	}
}

ptr指向的行内存排列应该是：
 B G R  B G R  。。。。。。。。。

  3*0+0      3*1+1
```

## 替代ROI方式

```
使img的部分区域红色增加100

int x=10,y=10,width=400,height=400;//感兴趣区域
IplImage* sub_img=cvCreateImageHeader(cvSize(width,height),img->depth,img->nChannels);
sub_img->origin=img->origin;
sub_img->widthStep=img->widthStep;
sub_img->imageData=img->imageData + img->widthStep*y + img->nChannels*x;
cvAddS(sub_img,cvScalar(0,0,100),sub_img);//让红通道增加100
cvReleaseImageHeader(&sub_img);

ROI只能串行处理，且不断设置和重置，该方式可以保持一副图像的多个子区域处于活动状态下。
```

## 拉伸变换

```
不能对一幅图进行两次拉伸，需要多次拉伸时，用多个dst接收src的变换，然后设置ROI进行合成即可。

CvPoint2D32f srcTir[4],dstTir[4];
CvMat *warp_mat=cvCreateMat(3,3,CV_32FC1);
IplImage *src=cvLoadImage("d:\\1.jpg");
IplImage *dst=cvCloneImage(src);
dst->origin=src->origin;
cvZero(dst);

///src中要进行变换的四个点坐标
srcTir[0].x=52;
srcTir[0].y=0;//LT
srcTir[1].x=src->width-52;
srcTir[1].y=0;//RT
srcTir[2].x=0;
srcTir[2].y=300;//LB
srcTir[3].x=src->width;
srcTir[3].y=300;//RB


//变换后的四个点坐标
dstTir[0].x=0;
dstTir[0].y=0;
dstTir[1].x=src->width;
dstTir[1].y=0;
dstTir[2].x=0;
dstTir[2].y=300;
dstTir[3].x=src->width;
dstTir[3].y=300;

cvGetPerspectiveTransform(srcTir,dstTir,warp_mat);
cvWarpPerspective(src,dst,warp_mat);


////对dst进行显示///////

cvReleaseImage(&dst);
cvReleaseMat(&warp_mat);
```

## 转换颜色类型

```
#pragma comment(lib,"opencv_objdetect248d")
#pragma comment(lib,"opencv_imgproc248d")

IplImage* gray = cvCreateImage(cvSize(img->width, img->height), 8, 1);
cvCvtColor(img, gray, CV_BGR2GRAY);
```
