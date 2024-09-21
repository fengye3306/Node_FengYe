## **拉伸、收缩、扭曲、旋转**  

调整大小本身也涉及到像素如何差值（放大）或合并（减少）问题。   

### 均匀调整  

均匀调整（Uniform Scaling）是指在不改变图像比例的情况下，缩放图像的过程。   
这个过程通常用于改变图像的大小（放大或缩小），并且保持原始图像的长宽比例。  
在计算机视觉领域，均匀调整广泛应用于图像处理和预处理，如在训练深度学习模型时调整输入图像的尺寸。  

在OpenCV中，cv::resize() 函数是实现均匀调整的主要工具。它支持多种插值方式，适用于不同的缩放需求。     

```OpenCV
/** OpenCV 插值方式（Interpolation Methods） */
enum
{
    /**
    描述：最近邻插值法。在缩放图像时直接选择最接近的新像素值。
    效果：计算速度快，但可能导致图像锯齿或像素化，适用于简单图像或不需要高质量的情况。
    */
    INTER_NEAREST = 0,
        /**< value = nearest neighbor interpolation */

    /**
    描述：双线性插值法。通过线性插值计算新像素值。
    效果：比最近邻插值平滑，适用于图像质量要求较高的情况。
    */
    INTER_LINEAR = 1,
        /**< value = bilinear interpolation */

    /**
    描述：区域插值法。基于图像区域平均值计算新像素值。
    效果：适合图像缩小，减少图像模糊效果。
    */
    INTER_AREA = 3,
        /**< value = resampling using pixel area relation */

    /**
    描述：双三次插值法。基于立方插值计算新像素值，考虑到更远的像素点。
    效果：在缩放较大的图像时效果最佳，适用于需要高质量插值的场景。
    */
    INTER_CUBIC = 2,
        /**< value = bicubic interpolation */

    /**
    描述：拉格朗日插值法。通过三次样条曲线计算新像素值。
    效果：比双线性插值和双三次插值更平滑，适用于图像锐化。
    */
    INTER_LANCZOS4 = 4,
        /**< value = Lanczos interpolation over 8x8 neighborhood */
};
```  

### 图像金字塔    

图像金字塔（Image Pyramids） 是一种多尺度图像表示方法，通过连续地对图像进行下采样（降低分辨率）或上采样（提高分辨率）来生成一系列的图像层次结构。金字塔结构广泛应用于计算机视觉中的图像分析、物体检测和图像压缩等任务。  

图像金字塔分为两类：高斯金字塔（Gaussian Pyramid） 和 拉普拉斯金字塔（Laplacian Pyramid）。高斯金字塔用于对图像进行平滑和下采样，而拉普拉斯金字塔则用于图像的边缘检测和细节增强。   

> 高斯金字塔 

高斯金字塔 是通过对图像应用高斯滤波器（Gaussian Filter）进行平滑处理，然后进行下采样来生成的。

* 生成过程：  

对原始图像应用高斯滤波器，得到平滑后的图像。
对平滑后的图像进行下采样（通常是每隔一个像素采样一次），得到低分辨率的图像。
重复以上步骤，对低分辨率图像继续应用高斯滤波器并下采样，生成更低分辨率的图像。  

* 数学推导：

设原始图像为 @I_0(x, y)@，高斯核为 @G(x, y)@，第一级金字塔图像 @I_1(x, y)@ 可以表示为：   

@@
I_1(x, y) = (I_0 * G) \downarrow 2
@@

其中 @*@ 表示卷积操作，@ \downarrow 2 @ 表示下采样（缩小一半尺寸）。  
这一过程重复进行，得到更高级别的图像 @I_n(x, y)@：

@@
I_n(x, y) = (I_{n-1} * G) \downarrow 2
@@

每次操作后，图像分辨率减少一半，金字塔层数增加一级。  


> 拉普拉斯金字塔     

拉普拉斯金字塔 是通过对高斯金字塔各级图像进行处理得到的，主要用于捕捉图像中的细节和边缘信息。

* 生成过程：  

首先通过高斯金字塔生成不同分辨率的图像。  
对高斯金字塔的每一层图像进行上采样（插值），然后与同层的高分辨率图像相减，得到拉普拉斯金字塔的各级图像。  

* 数学推导：  

设 @L_n(x, y)@ 是拉普拉斯金字塔第 @n@ 级的图像，@I_n(x, y)@ 是高斯金字塔第 @n@ 级的图像，@I_{n+1}(x, y)@ 是高斯金字塔第 @n+1@ 级的图像。拉普拉斯金字塔的生成公式为：  

@@
L_n(x, y) = I_n(x, y) - \text{Upsample}(I_{n+1}(x, y))
@@

其中 @\text{Upsample}()@ 表示上采样操作。通过上采样，我们将低分辨率的图像扩展回原始分辨率，然后与原始图像相减，得到细节信息。

> OpenCV示例    

在OpenCV中，可以使用`cv::pyrDown`和`cv::pyrUp`函数生成高斯金字塔和拉普拉斯金字塔。   

```cpp
/** 生成高斯金字塔：
*/
cv::Mat src = cv::imread("image.jpg");  // 加载图像
cv::Mat gaussian_pyramid;

// 对图像进行降采样，生成金字塔
cv::pyrDown(src, gaussian_pyramid);
```

```cpp
/** 生成拉普拉斯金字塔：
*/
cv::Mat src = cv::imread("image.jpg");  // 加载图像
cv::Mat gaussian_pyramid, laplacian_pyramid;

// 生成高斯金字塔
cv::pyrDown(src, gaussian_pyramid);

// 上采样并计算拉普拉斯金字塔
cv::pyrUp(gaussian_pyramid, laplacian_pyramid);
laplacian_pyramid = src - laplacian_pyramid;  // 计算细节信息
```

### 仿射变换

仿射变换（Affine Transformation） 是一种保持点、直线之间关系不变的线性变换，包括平移、旋转、缩放、剪切和翻转等操作。  
仿射变换可以用一个线性变换矩阵和一个位移向量来描述，其本质是线性代数中的矩阵运算，通过左乘一个矩阵来实现坐标系转换。   

> 二维空间的线性变换 数学推导   

仿射变换将原始坐标点 @\mathbf{P}(x, y)@ 转换为新坐标点 @\mathbf{P'}(x', y')@，其数学表达式为：

@@
\mathbf{P'} = \mathbf{A} \cdot \mathbf{P} + \mathbf{B}
@@

其中，@ \mathbf{A} @ 是一个 2x2 的线性变换矩阵，@ \mathbf{B} @ 是一个平移向量，@ \mathbf{P} @ 和 @ \mathbf{P'} @ 是齐次坐标表示的二维向量。

齐次坐标表示：
@@
\mathbf{P} = \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
\quad
\mathbf{P'} = \begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix}
@@

矩阵 @ \mathbf{A} @ 的形式为：
@@
\mathbf{A} = \begin{bmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{bmatrix}
@@

平移向量 @ \mathbf{B} @ 的形式为：
@@
\mathbf{B} = \begin{bmatrix} b_1 \\ b_2 \end{bmatrix}
@@

因此，仿射变换的完整表达式为：
@@
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} a_{11} & a_{12} & b_1 \\ a_{21} & a_{22} & b_2 \\ 0 & 0 & 1 \end{bmatrix} \cdot \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
@@

本质上来说 @\mathbf{P'}@ 是通过对原坐标 @\mathbf{P}@ 进行矩阵 @\mathbf{A}@ 的线性变换后，再加上一个平移 @\mathbf{B}@ 所得到。   

> OpenCV,密集仿射变换 与 稀疏仿射变换

在 OpenCV 中，仿射变换可以分为密集仿射变换和稀疏仿射变换两类。

1. `cv::warpAffine`—— 密集仿射变换

密集仿射变换是用于对整个图像进行仿射变换的函数，属于密集仿射变换，其本质对图像的每一个像素点进行变换。

```cpp
/**
void cv::warpAffine (
    InputArray src, 
    OutputArray dst, 
    InputArray M,                        // 2x3 仿射变换矩阵
    Size dsize,                          // 输出图像的大小
    int flags = INTER_LINEAR,            // 插值方式（默认是线性插值）
    int borderMode = BORDER_CONSTANT,    // 边界处理方式
    const Scalar &borderValue = Scalar() // 若使用填充，则填充像素为
    )
*/

cv::Mat src = cv::imread("image.jpg");
cv::Mat dst;
// 2x3 仿射变换矩阵：平移（50, 50）
cv::Mat M = (cv::Mat_<double>(2, 3) << 1, 0, 50, 0, 1, 50);
// 执行仿射变换
cv::warpAffine(src, dst, M, src.size());
cv::imwrite("output.jpg", dst);
```


2. `cv::transform`—— 稀疏仿射变换

稀疏仿射变换用于对图像中的一组点进行仿射变换的函数。与 *密集仿射变换* 不同的是，**它只对给定的点进行变换，而不是整个图像**。  

```cpp
/**
void cv::transform(
    InputArray src,     // 输入点集（可以是二维点集）。
    OutputArray dst,    // 输出点集。
    InputArray mtx      // 变换矩阵（2x3 或 3x3）。
    )
*/

std::vector<cv::Point2f> srcPoints = {cv::Point2f(0, 0), cv::Point2f(1, 0), cv::Point2f(0, 1)};
std::vector<cv::Point2f> dstPoints;

// 定义一个缩放矩阵
cv::Mat M = (cv::Mat_<double>(2, 3) << 2, 0, 0, 0, 2, 0);

// 对点集进行变换
cv::transform(srcPoints, dstPoints, M);

// 打印变换后的点
for (auto& pt : dstPoints) {
    std::cout << pt << std::endl;
}
```

> OpenCV 求解仿射变换矩阵  

1. `cv::invertAffineTransform` —— 逆仿射变换

`cv::invertAffineTransform` 用于计算仿射变换矩阵的**逆矩阵**。

```cpp
/**
void cv::invertAffineTransform(
    InputArray M,   // 输入仿射变换矩阵（2x3）。
    OutputArray iM  // 输出的逆仿射变换矩阵
    )
*/
cv::Mat M = (cv::Mat_<double>(2, 3) << 1, 0, 50, 0, 1, 50);
cv::Mat iM;

// 计算逆仿射变换矩阵
cv::invertAffineTransform(M, iM);

// 打印逆矩阵
std::cout << "Inverse Matrix:\n" << iM << std::endl;
```

2. `cv::getAffineTransform` —— 获取仿射变换矩阵  

根据原始图像中的三点和目标图像中的对应三点来计算仿射变换矩阵。  

```cpp
/** 
Mat cv::getAffineTransform(
    const Point2f src[],    // 源图像中的三个点
    const Point2f dst[]     // 目标图像中的三个点
)
*/

cv::Point2f srcTri[3];
cv::Point2f dstTri[3];

srcTri[0] = cv::Point2f(0, 0);
srcTri[1] = cv::Point2f(1, 0);
srcTri[2] = cv::Point2f(0, 1);

dstTri[0] = cv::Point2f(0, 0);
dstTri[1] = cv::Point2f(1, 1);
dstTri[2] = cv::Point2f(-1, 1);

// 计算仿射变换矩阵
cv::Mat M = cv::getAffineTransform(srcTri, dstTri);

```



3. `cv::getRotationMatrix2D` —— 获取旋转矩阵

```cpp
Mat cv::getRotationMatrix2D(
    Point2f center,     // 旋转中心点
    double angle,       // 旋转角度
    double scale        // 缩放因子
    )

cv::Mat src = cv::imread("image.jpg");
cv::Mat dst;

// 计算旋转矩阵
cv::Point2f center(src.cols/2.0, src.rows/2.0);
double angle = 45.0;
double scale = 1.0;
cv::Mat M = cv::getRotationMatrix2D(center, angle, scale);

// 执行仿射变换
cv::warpAffine(src, dst, M, src.size());
cv::imwrite("rotated.jpg", dst);
```


### 透视变换

透视变换是一种几何变换，它可以将图像从一个平面转换到另一个平面，改变图像的形状和比例，使得看起来像是从另一个角度观察的。  
透视变换就像你把一张纸从一个角度拉伸、旋转或翻转，看上去这张纸的形状变了，但它其实还是原来的那张纸，只是你看它的角度不同了。    

透视变换并不是一种线性变换，其允许图像中的直线保持直线，但平行线不再保持平行。这使得图像看起来有深度或距离感，类似于在现实生活中透视的效果。    

![透视变换](./image/ImageTransformations/01.png ':size=WIDTHxHEIGHT')

**拍摄修复**   
你用手机拍摄文档或书籍页面时，图片可能会有一定的角度倾斜，导致文档的四边不是矩形，而是一个梯形，通过透视变换，可以将这个梯形校正为一个标准的矩形，使得文档看起来像是从正上方拍摄的，便于阅读和进一步处理。  

**图像拼接与全景图**    
在生成全景图像时，需要将多张图像拼接在一起。这些图像通常是从不同角度拍摄的，因此需要对齐。  
通过透视变换，可以将不同角度拍摄的图像变换到同一个平面上，使它们能够无缝拼接。   

将图片由一个观察视角纠正到另一个观察视角就是透视变换的本质，如上都是将透视进行正变换。     
**图像中的虚拟广告植入**  
在体育赛事直播中，有时需要将广告植入到比赛场地的特定区域，比如足球场或篮球场的地面上。  
通过透视变换，可以将二维的广告图像变换为与地面视角一致的图像，使得广告看起来像是实际存在于场地上。  

> 透视变换数学模型

透视变换在数学上是一种 射影变换 (Projective Transformation)，射影变换属于 数学中的射影几何（Projective Geometry）学科。    
射影几何 是几何学的一个分支，研究的是几何图形在射影变换下的性质。 

@@
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix}
=
\begin{bmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{bmatrix}
\cdot
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
@@

> OpenCV透视变换  

1. `cv::getPerspectiveTransform`——计算透视变换矩阵  

这是一个将4个点的对应关系转化为3x3透视矩阵的函数。输入为原图像中的4个点和目标图像中的4个点，输出为透视变换矩阵。

```cpp

cv::Point2f srcPoints[4] = { ... }; // 原图像的四个点
cv::Point2f dstPoints[4] = { ... }; // 目标图像的四个点
cv::Mat perspectiveMatrix = cv::getPerspectiveTransform(srcPoints, dstPoints);

```

2. `cv::warpPerspective`——将图像进行透视变换。  

这是实际执行透视变换的函数，使用由 cv::getPerspectiveTransform 计算的矩阵。

```cpp
cv::Mat srcImage = cv::imread("image.jpg"); // 原图像
cv::Mat dstImage;
cv::warpPerspective(srcImage, dstImage, perspectiveMatrix, cv::Size(width, height));
```

3. `cv::invertPerspectiveTransform`——计算透视变换的逆矩阵。  

这在需要将变换后的图像点映射回原图像时很有用。

```cpp
cv::Mat inverseMatrix;
cv::invert(perspectiveMatrix, inverseMatrix, cv::DECOMP_LU);
```

## **通用变换**

### 极坐标映射  

**极坐标系在某些情况下可以显著简化计算**


极坐标系通过原点（极点）以及角度 @θ@ 和距离 @r@ 来表示点的位置。  
与笛卡尔坐标系相比，极坐标系在处理旋转变换时具有显著优势，因为旋转变换只影响角度 @θ@，而半径 @r@ 不受影响。这使得在进行旋转变换时，极坐标系能够更加自然和高效地表示图像中的点。  

在笛卡尔坐标系中，旋转变换会导致图像中的点位置发生变化，需要重新计算这些点的新坐标。  
这通常涉及复杂的插值计算，因为旋转后的坐标点可能不会落在原始像素的整数位置上。因此，为了确定旋转后像素的值，必须使用插值技术，这增加了计算的复杂性和计算量。  

### LogPolar极坐标系    

“LogPolar” 是一种特殊的极坐标表示方式，但它并不完全等同于普通的极坐标系统。    

* 普通极坐标系（Polar Coordinates）    
表示方式：通过原点（极点）、一个角度 @θ@ 和一个半径 @r@ 来表示点的位置。  
坐标：用 @ (r, θ) @ 表示，其中 @ r @ 是到原点的距离，@ θ @ 是与参考方向的夹角。  

* 对数极坐标系（Log-Polar Coordinates）  
表示方式：将极坐标系统中的半径 @r@ 使用对数变换来表示，即 @ \log(r) @。
坐标：用 @ (\log(r), θ) @ 表示，其中 @ \log(r) @ 是半径 @r@ 的对数值，@ θ @ 仍然是与参考方向的夹角。    
变换的好处：这个变换的好处是，它将原本以指数方式增长的半径 @r@ 转换为对数尺度，从而使得处理尺度变化（如图像缩放）更加方便，并且在计算上更为稳定。      


```cpp
/**
void cv::logPolar(
    InputArray src,     // 输入图像
    OutputArray dst,    // 输出图像，类型为 cv::Mat，存储 LogPolar 坐标系下的图像
    Point2f center,     // 图像中心点，在 LogPolar 坐标系中通常是图像的中心。
    double M,           // 尺度因子，用于控制 LogPolar 图像的大小。
    int flags = INTER_LINEAR + WARP_FILL_OUTLIERS //插值方式和填充标志
);
*/
cv::Mat src = cv::imread("input.jpg", cv::IMREAD_GRAYSCALE);
cv::Mat dst;
cv::Point2f center(src.cols / 2, src.rows / 2);
double scale = 40;  // 比例因子

cv::logPolar(src, dst, center, scale, cv::INTER_LINEAR + cv::WARP_FILL_OUTLIERS);
cv::imwrite("logpolar.jpg", dst);

```

1. Cartesian坐标系转换为LogPolar坐标系

```cpp
cv::Mat src = cv::imread("input.jpg", cv::IMREAD_GRAYSCALE);
cv::Mat dst;
cv::Point2f center(src.cols / 2, src.rows / 2);
double scale = 40;  // 比例因子

cv::logPolar(src, dst, center, scale, cv::INTER_LINEAR + cv::WARP_FILL_OUTLIERS);
cv::imwrite("logpolar.jpg", dst);
```
2. LogPolar坐标系转换为Cartesian坐标系

```cpp
/**
void cv::linearPolar(
    InputArray src, 
    OutputArray dst, 
    Point2f center, 
    double M, 
    int flags = INTER_LINEAR + WARP_FILL_OUTLIERS
);
*/
cv::Mat src = cv::imread("logpolar.jpg", cv::IMREAD_GRAYSCALE);
cv::Mat dst;
cv::Point2f center(src.cols / 2, src.rows / 2);
double scale = 40;  // 比例因子

cv::linearPolar(src, dst, center, scale, cv::INTER_LINEAR + cv::WARP_FILL_OUTLIERS);
cv::imwrite("cartesian.jpg", dst);
```


## **图像修复**

图像因为噪声而破损，镜头上可能存在灰尘或是水渍，或者图像的一部分被损坏。  

如下，对因为文字覆盖而损坏的图像。   
![图像修复](./image/ImageTransformations/01.png ':size=WIDTHxHEIGHT')


### 图像修复

> 当前图像修复技术：  

* 传统方法：

使用算法如 cv::inpaint 来填补图像中的缺失区域。  
传统方法适用于简单的修复任务，但在处理复杂的纹理和结构时可能效果不佳。  

* AI 和深度学习方法： 

基于深度学习的修复技术（如卷积神经网络）可以更好地处理复杂的图像修复任务。
AI 方法可以学习图像的高级特征和结构，因此在修复较复杂的图像细节时通常表现更好。


> OpenCV图像修复

cv::inpaint 是 OpenCV 提供的一个函数，用于图像修复（去除图像中的瑕疵）。它通过利用图像的周围像素信息来填补缺失或损坏的区域，从而使图像看起来更完整。以下是 cv::inpaint 接口的详细说明及调用实例： 

```cpp
// void cv::inpaint(
//     InputArray src, 
//     InputArray inpaintMask, 
//     OutputArray dst, 
//     double inpaintRadius, 
//     int flags
// );

#include <opencv2/opencv.hpp>

int main() {
    // 读取图像
    cv::Mat src = cv::imread("image.jpg");
    
    // 创建掩模图像
    cv::Mat inpaintMask = cv::Mat::zeros(src.size(), CV_8UC1);
    
    // 设定掩模区域 (例如，圆形区域)
    cv::circle(inpaintMask, cv::Point(100, 100), 20, cv::Scalar(255), -1);
    
    // 创建输出图像
    cv::Mat dst;
    
    // 调用 inpaint 函数进行图像修复
    cv::inpaint(src, inpaintMask, dst, 3, cv::INPAINT_TELEA);
    
    // 保存修复后的图像
    cv::imwrite("repaired_image.jpg", dst);
    
    return 0;
}
```

