## **离散傅里叶变换**

傅里叶变换是将时间或空间域的信号转换为频率域表示的工具。  
离散傅里叶变换（DFT）是傅里叶变换的离散版本，用于分析和处理离散信号或图像。OpenCV 提供了 cv::dft 和 cv::idft 函数来进行离散傅里叶变换及其逆变换。      

图像也可以被看作是信号。具体来说，图像是由像素组成的二维数据，每个像素的亮度或颜色值代表了图像在某一位置的信号强度。因此，图像的傅里叶变换是一种将图像从空间域（像素值随空间变化）转换到频率域的数学工具。    
空间域 和 时域 是类似的概念，但针对不同的数据类型使用。   
* 时域：通常指随时间变化的信号，比如声音信号、传感器数据等。在时域中，我们关注的是信号如何随时间变化。
* 空间域：指随空间位置变化的信号，比如图像。在空间域中，我们关注的是图像中各个位置（像素）上的亮度或颜色值如何变化   

之后，对于空间域的DFT变换也是将连续信号打散为一系列正弦波与余弦波，本质任然如此。   
在OpenCV中，cv::Mat 是一个二维矩阵，它不仅仅用于存储图像，还可以用来存储其他类型的数据，包括傅里叶变换的结果。  
傅里叶变换后的结果是一个复数矩阵，每个复数元素包含两个部分：  
* 实部（real part）: 对应着余弦波的振幅
* 虚部（imaginary part）: 对应着正弦波的振幅
实部和虚部组合在一起就描述了图像中每一个频域分量的强度和相位。     

> 二维离散傅里叶变换（DFT）   

一张灰度图，其是由不同的明暗变化构成的。我们可以将之分割为小格子，再给小格子赋予一个数值。  
将灰度图平铺于一个三维坐标系中，越亮的地方数值越高，越暗的地方数值越低，链接起来就能获得一个连绵起伏的**函数图像**了。    
这其实是一个**二元函数** 

![图像函数化](./image/ImaeAnalysis/01.png ':size=WIDTHxHEIGHT')

二维离散傅里叶变换：
@@
F(u, v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x, y) \cdot e^{-2\pi i \left(\frac{ux}{M} + \frac{vy}{N}\right)}
@@


### cv::dft 离散傅里叶变换  

OpenCV 提供了`cv::dft` 函数用于实现离散傅里叶变换。它既可以处理实数也可以处理复数输入。     
对于产出的结果图像，图像中心为零频率，低频区域代表图像的主要形状，高频区域代表边缘和细节。   
高频部分表征的是图像中变化快速的区域，如边缘和噪点。   
低频区域代表图像的主要形状。     

* **通过使用高通滤波器可以进行边缘检测。**  
在频域中，通过创建一个高通滤波器来屏蔽低频成分。高通滤波器可以是一个简单的二维阵列，中心为低值（接近0），边缘为高值（接近1），这样就只保留了图像中的高频信息，即边缘信息。     
应用完高通滤波器后，需要将结果转换回空间域以查看实际的图像。这通过逆傅里叶变换完成，将频域的数据重新构造成可视化的图像形式。  

* **图像去噪**        
在频域中，噪声通常表现为高频信息。通过应用低通滤波器，可以去除这些高频噪声，从而平滑图像。  

* **图像压缩**    
在进行傅里叶变换后，许多图像主要形状会集中在频域的低频区域。通过只保留这部分重要的频率成分（并且可能量化这些频率成分），可以有效压缩图像，减少存储需求。     
 
* **图像融合，添加隐形水印**     
隐形水印的目的是在图像中嵌入不可见的信息（如版权标记或其他标识符），而不影响图像的视觉质量。在频域中添加水印通常意味着更难被检测和移除。      

水印可以是一个文本、另一幅图像或任何类型的数字信息。宿主图像是你想要嵌入水印的原始图像。
对宿主图像进行傅里叶变换，转换到频域。   
确定嵌入水印的频域区域。常见的选择是图像的中频区域，因为这部分既不是图像的最明显的低频背景信息，也不是容易被压缩和噪声影响的高频细节部分。     
   
对于每个水印位，选择一个频率成分。  
如果水印位是1，就增加该频率成分相位的一个小量（例如 π/4）。  
如果水印位是0，可以选择不改变相位或者减少相同的量。  

在完成水印的嵌入后，对修改后的频域数据应用逆傅里叶变换，将图像转换回空间域。   

对于隐形频域水印的检测如下    
要检测水印，需要再次对图像进行傅里叶变换，并在相同的频率成分上检查相位的变化。   
通过比较变化前后的相位差，可以确定水印位的值。  
 

```cpp
/*
void dft(
  // 输入数组，可以是实数或复数
  cv::InputArray src,
  // 输出数组，其大小和类型取决于输入数组
  cv::OutputArray dst,
  // 转换标志，如 cv::DFT_INVERSE，cv::DFT_SCALE 等 
  int flags = 0, 
  // 当这个值为非零时，函数会假设只有 nonzeroRows 行包含非零
  int nonzeroRows = 0
);
*/

#include <opencv2/opencv.hpp>
using namespace cv;

int main() {
    // 加载图像
    Mat img = imread("path_to_image", IMREAD_GRAYSCALE);
    img.convertTo(img, CV_32F); // 转换为浮点类型

    // 扩展图像到最佳尺寸
    Mat padded;
    int m = getOptimalDFTSize(img.rows);
    int n = getOptimalDFTSize(img.cols);
    copyMakeBorder(img, padded, 0, m - img.rows, 0, n - img.cols, BORDER_CONSTANT, Scalar::all(0));

    // 创建两个通道，一个为图像，一个为全0
    Mat planes[] = {Mat_<float>(padded), Mat::zeros(padded.size(), CV_32F)};
    Mat complexImg;
    merge(planes, 2, complexImg);

    // 应用DFT
    dft(complexImg, complexImg);

    // 将复数转换为幅度
    split(complexImg, planes);
    magnitude(planes[0], planes[1], planes[0]);
    Mat magImg = planes[0];

    // 转换到对数尺度
    magImg += Scalar::all(1);
    log(magImg, magImg);

    // 裁剪和重排频谱图
    magImg = magImg(Rect(0, 0, magImg.cols & -2, magImg.rows & -2));
    int cx = magImg.cols/2;
    int cy = magImg.rows/2;

    // 重新排列象限
    Mat tmp;
    Mat q0(magImg, Rect(0, 0, cx, cy));
    Mat q1(magImg, Rect(cx, 0, cx, cy));
    Mat q2(magImg, Rect(0, cy, cx, cy));
    Mat q3(magImg, Rect(cx, cy, cx, cy));

    q0.copyTo(tmp);
    q3.copyTo(q0);
    tmp.copyTo(q3);
    q1.copyTo(tmp);
    q2.copyTo(q1);
    tmp.copyTo(q2);

    normalize(magImg, magImg, 0, 255, NORM_MINMAX);

    // 显示结果
    imshow("Input Image", img);  // 显示原图
    imshow("spectrum magnitude", magImg); // 显示频谱图
    waitKey();

    return 0;
}
```

### cv::idft 离散傅里叶逆变换  

```cpp
#include <opencv2/opencv.hpp>

int main() {
    // 假设已有一个频域图像 complexImg
    cv::Mat complexImg;

    // 创建输出矩阵
    cv::Mat inverseTransform;

    // 执行离散傅里叶逆变换
    cv::idft(complexImg, inverseTransform, cv::DFT_SCALE | cv::DFT_REAL_OUTPUT);

    // 将结果转换为可视化的格式
    cv::Mat restoredImage;
    inverseTransform.convertTo(restoredImage, CV_8U);

    // 显示结果
    cv::imshow("Restored Image", restoredImage);
    cv::waitKey(0);

    return 0;
}
```

## **积分图**

积分图时一种允许子区域快速求和的数据结构，这种求和在很多方面都很有用，值得一提的是haar小波的计算，它用于人脸识别和类似的算法。Opencv支持积分图的三种变体，分别是总和、平方求和以及倾斜求和。每种情况的结果图像在图像的每个方向上都加1之后，与原始图像的大小相同。     

通过积分图，你可以对图像的任意直立或“倾斜”的矩形区域求和、平均以及标准差。即使目标区域大小不确定，也可以高效进行快速模糊、近似梯度、求均值以及标准差，即使对可变大小的窗口，也可以执行快速块相关。   

> 为什么积分图可以实现区块快速求和？   

积分图的主要思想为：积分图图像中的每一个像素点 @f(x, y)@ 存储从@ (0, 0)@ 到 @f(x, y) @所有像素的累积和。    
具体来说，积分图中在位置 @(x, y) @的值是原始图像从左上角开始到@ (x, y)@ 形成的矩形区域内所有像素值的和。   
对于这种数据结构，假使需要求解任意矩形区域@(x1, y1)@到@(x2,y2)@的像素总和，只需要四个元素。     

![积分图](./image/ImaeAnalysis/02.png ':size=WIDTHxHEIGHT')

对于矩形区块，可以以@O（1）@ 复杂度到这个区域的像素总和，而不需要进行多次循环求和。这个求解的时间复杂度为
@@
\text{Sum} = ii(x2, y2) - ii(x2, y1-1) - ii(x1-1, y2) + ii(x1-1, y1-1)
@@

### cv::integral标准求和积分  

```cpp
/**
void integral( 
  InputArray src, 
  OutputArray sum, 
  int sdepth = -1  // 元素数据类型深度
);
*/

 // 计算积分图
integral(img, sum, CV_64F);  // 显式指定使用64位浮点数
```  

形参`sdepth`决定积分图像中元素数据类型深度，更高的数据类型深度（如 CV_64F）虽然可以提供更高的精度和容量，但同时也会增加计算资源和内存的消耗。而过低的数据类型深度则产生数据溢出风险。     
值`-1`表示自动求解元素数据类型深度。   

### cv::integral平方求和积分
平方求和积分图为图像中每个位置 (x, y) 提供从 (0, 0) 到 (x, y) 所有像素的平方值累积和。这对于计算局部窗口的方差和标准差尤其有用。

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat img = cv::imread("path_to_image.jpg", cv::IMREAD_GRAYSCALE); // 加载灰度图像
    cv::Mat sum, sqsum;

    // 计算标准积分图和平方积分图
    cv::integral(img, sum, sqsum, CV_64F); // 显式指定使用64位浮点数

    return 0;
}
```
### cv::integral倾斜求和积分

倾斜求和积分图（也称为旋转积分图）计算从 (0, 0) 到每个位置 (x, y) 经过45度旋转的所有像素的累积和。这对于检测图像中的倾斜特征和模式匹配非常有用。  

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat img = cv::imread("path_to_image.jpg", cv::IMREAD_GRAYSCALE); // 加载灰度图像
    cv::Mat sum, sqsum, tilted;

    // 计算标准积分图、平方积分图和倾斜积分图
    cv::integral(img, sum, sqsum, tilted, CV_64F); // 显式指定使用64位浮点数

    return 0;
}
```

## **Canny边缘检测**    

Canny边缘检测算法于1986年由John F. Canny提出，这种算法特别注重实现三个主要的目标：低错误率、明确的边缘定位和最小化多响应。   

> Canny算法的实现   

1. 噪声去除：   

由于边缘检测容易受到图像噪声的干扰，Canny算法首先使用高斯滤波器平滑图像以消除噪声。    

2. 计算梯度幅度和方向：   

使用一对一维的Sobel滤波器来计算图像在水平和垂直方向上的一阶导数（即梯度幅度和方向）。这些导数用于找到潜在的边缘。

3. 非极大值抑制：   

在梯度幅度的图像中实施非极大值抑制，这个步骤帮助去除非边缘的点或抑制（削弱）边缘响应不是最强的点，仅保留梯度幅度最大的作为边缘点。

4. 双阈值检测和边缘连接：    

应用两个阈值（一个高阈值和一个低阈值）来确定真正的边缘。大于高阈值的强边缘直接确定为边缘点，小于低阈值的点被抛弃。介于两者之间的则根据其连接性（是否与强边缘点相连）决定是否保留。
这个步骤确保了边缘的完整性，连接分散的边缘片段。       

## **Hough变换**

### Hough线变换

### Hough圆变换

## **距离变换** 

### 无标记距离变换

### 有标记距离变换

## **分割**

### 漫水填充

### 分水岭算法

### Grabcuts算法

### Mean-Shift分割算法

