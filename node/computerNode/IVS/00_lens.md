# 镜头  

工业相机镜头是光学接收对象，接收并对其进行调整，从而实现光学成像。镜头质量y直接影响成像质量。  


## 工业镜头结构

![镜头结构](./Img/02_lens/01.png ':size=WIDTHxHEIGHT')

1. D - 光阑 (Aperture)     

光阑是镜头中的一个装置，用于控制进入镜头的光线量。通常通过调整光圈的大小来改变光线的通光量。光阑的作用类似于人眼的瞳孔，光圈越大，进入的光线越多，图像亮度越高；光圈越小，光线减少，图像会变暗。光阑还影响景深（物体从前到后清晰的范围）。  

2. ENP - 入瞳 (Entrance Pupil)  

入瞳是从物体方向看进入镜头光阑的有效孔径。简单来说，入瞳是光线从物体进入镜头时“看到”的光圈位置。入瞳的大小决定了进入镜头的光线角度以及整个光学系统的光通量（进入镜头的总光量）。  

3. EXP - 出瞳 (Exit Pupil)   

出瞳是从图像传感器方向看出去时，光阑的虚拟位置或大小。它是经过镜头系统投射到图像平面上的光阑的像。出瞳决定了光线离开镜头时形成的图像照亮传感器的情况。在一些应用场景中，出瞳的大小和位置对成像的均匀性和视场有很大影响。

## 工业镜头分类  

* 按照焦距划分    
定焦镜头与变焦镜头    
* 按照光圈划分    
固定光圈与可变光圈    
* 按照接口（与相机的连接接口）划分     
C接口、CS接口、F接口    
* 按照倍数划分     
定倍镜头、连续变倍镜头      

机器视觉常用镜头包括：   
FA镜头、远心镜头和工业显微镜    

## 工业镜头基本参数  

![镜头参数](./Img/02_lens/02.png ':size=WIDTHxHEIGHT')


>  视场（FOV）（Field of Viw）     

也叫 **视野范围**
指观测物体的可视范围，也就是充满相机采集芯片的物体部分。   

> 工作距离（WD）（Working Distance）   

指从镜头**前部到受检验物体的距离**，即清晰成像的表面距离   

> 分辨率   

图像系统可以测到的受检测物体 上 最小可分辨特征尺寸。   
多数情况下，视野越小，分辨率越好。   

> 景深（DOF）（Depth of View）   

物体离最佳焦点较近或较远时，镜头所保持所需分辨率的能力   

> 焦距：  

指光学系统中衡量光的聚集或发散的度量方式，指从透镜的光心到广聚集之焦点的距离。   
焦距越小，景深越大，畸变越大，渐晕现象越严重。图像边缘的照度降低。   

> 感光芯片尺寸  

指相机感光芯片的有效区域尺寸，一般指水平尺寸。   

## 工业镜头放大倍数    

![工业镜头放大倍数](./Img/02_lens/03.png ':size=WIDTHxHEIGHT')  

## 工业镜头选型公式  

![工业镜头选型公式](./Img/02_lens/04.png ':size=WIDTHxHEIGHT')  


## CCD尺寸影响镜头选型  

CCD尺寸（或CMOS）就是感光芯片尺寸，在选择镜头时，必须确保镜头的成像圈可以覆盖感光芯片的整个有效区域，以避免图像边缘的暗角或模糊。   
镜头的焦距、光圈以及成像质量等因素都需要根据感光芯片的尺寸进行调整，以获得最佳的成像效果。  

![CCD尺寸影响镜头选型](./Img/02_lens/05.png ':size=WIDTHxHEIGHT')    

为了保证画面的整体可用性，选用镜头的像面尺寸应大于相机芯片的对角线尺寸（也被称为靶面），否则会出现边缘暗角/黑角等情况，影响使用。     

## 镜头选型注意事项  

1. 视野范围、光学放大倍数以及期望工作距离：  
在选择镜头时，选择比被测物视野稍大的镜头，以便进行运动控制。   
2. 景深需求：   
对景深有要求的项目应尽可能使用小光圈；选择放大倍率镜头时，经可能选低倍率镜头；如果项目要求严苛，则尽可能选用高景深的尖端镜头   
3. 芯片大小与相机接口：   
例如2/3英寸镜头支持最大的相机靶面位2/3英寸，其不能支持1英寸以上的工业相机。  
4. 注意光源配合   
5. 注意可安装空间  
6. 注意是否需要加接圈   

## 镜头选型步骤  

1. 先进行相机选型，相机选型后才能确定感光芯片的大小和视场大小  
2. 计算光学放大倍率
3. 确定工作距离 
4. 计算焦距，查找目录找出符合的型号   
5. 观察景深、分辨率是否满足要求
6. 确定镜头接口和最大兼容CCD无问题  


## 工业镜头选型实例   

**检测要求**

* 视野范围：12mm * 9mm
* 镜头前端距离被测物体距离：60mm
* 选用相机：320万像素相机（分辨率2048*1536，像素）

**选型步骤**

1. 计算感光芯片的实际长度          


@@
\text{Sensor长度} = \frac{2048 \times 3.45}{1000} = 7.0656 \text{ mm}
@@

2. 计算感光芯片的实际宽度

@@
\text{Sensor高度} = \frac{1536 \times 3.45}{1000} = 5.2992 \text{ mm}
@@

3. 计算光学放大倍数   

光学放大倍数计算公式为：  
@@
\text{Optical Magnification} = \frac{\text{Sensor Length}}{\text{Field of View Length}}
@@
带入公式可得：    
@@
\text{Optical Magnification} = \frac{7.0656}{12} = 0.5888
@@

4. 焦距  

@@
f = 0.5888*60 = 35.328 \text{mm}
@@

* 实际光学放大倍数   

@@
35/60 = 0.5833  
@@

* 实际视野长度  

@@
7.0656/0.5833 = 12.113\text{mm}
@@ 

* 实际视野宽度 

@@
5.2992/0.5833 = 9.085\text{mm}
@@ 

* 单像素精度  

@@
12.113/2048 = 0.0059\text{mm}
@@ 


**选型结果**

35mm镜头搭配320万相机，可以达到12.113mm*9.085mm视野，单像素精度5.9um。   
