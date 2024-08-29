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

> 图像函数化   

一张灰度图，其是由不同的明暗变化构成的。我们可以将之分割为小格子，再给小格子赋予一个数值。  
将灰度图平铺于一个三维坐标系中，越亮的地方数值越高，越暗的地方数值越低，链接起来就能获得一个连绵起伏的**函数图像**了。    
这其实是一个**二元函数** 



![图像函数化](./image/ImaeAnalysis/01.png ':size=WIDTHxHEIGHT')



### cv::dft 离散傅里叶变换  

```cpp
/**
void cv::dft(
    InputArray src,     // 输入图像或信号，cv::Mat
    OutputArray dst,    // 输出的变换结果，通常为 CV_32F 或 CV_64F 类型的 cv::Mat 
    /
     用于指定变换的类型，
      *DFT_INVERSE（逆变换）
      *DFT_REAL_OUTPUT（只输出实部）
      *DFT_COMPLEX_OUTPUT（输出复数结果）
    /
    int flags = 0,      
    int nonzeroRows = 0 // 用于优化的参数，指定输入中非零的行数。对于大部分情况，该参数应设置为0
);
*/

// 创建一个 8x8 的图像
cv::Mat img = cv::Mat::zeros(8, 8, CV_32F);
img.at<float>(3, 3) = 255.0;

// 执行 DFT
cv::Mat dft_result;
cv::dft(img, dft_result, cv::DFT_COMPLEX_OUTPUT);

// 结果为复数形式，需要将频谱平移
cv::Mat planes[2];
cv::split(dft_result, planes);
cv::magnitude(planes[0], planes[1], planes[0]); // 计算幅度
cv::Mat mag_img = planes[0];

// 对数尺度变换以增强对比度
mag_img += cv::Scalar::all(1);
cv::log(mag_img, mag_img);
```

### cv::idft 离散傅里叶逆变换  

### cv::mulSpectrums频域乘法  

### 使用傅里叶变换卷积

### cv::dct离散余弦变换  

### cv::idct离散余弦逆变换   

## **积分图**

### cv::integral标准求和积分

### cv::integral平方求和积分

### cv::integral倾斜求和积分

## **Canny边缘检测**  

## Hough变换

### Hough线变换

### Hough圆变换

## 距离变换 

### 无标记距离变换

### 有标记距离变换

## 分割

### 漫水填充

### 分水岭算法

### Grabcuts算法

### Mean-Shift分割算法

