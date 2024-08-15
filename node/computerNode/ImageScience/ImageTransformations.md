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


## **通用变换**

### 极坐标映射
### LogPolar
### 任意映射

## **图像修复**

### 图像修复
### 去噪

## **直方图均衡化**


