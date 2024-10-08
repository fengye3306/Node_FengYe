# 傅里叶变换  

```link
# TITLE:  傅里叶分析之掐死教程
https://zhuanlan.zhihu.com/p/19763358
```

```link
# TITLE:  合集·傅里叶变换从零到一
https://space.bilibili.com/2008799191/channel/collectiondetail?sid=990857
```

```link
# TITLE:  合集·【傅里叶级数与傅里叶变换】_DR_CAN合集
https://space.bilibili.com/230105574/channel/collectiondetail?sid=1814755
```

## 从角频率到频域

*什么是角频率，其和正弦函数有什么关系？由之，我们将推导出频域。*      

> 频率

我们平时所说的*频率*就是一秒钟之内可以做多少次循环重复的动作   
例如，我一秒钟可以拍掌6次，我的拍掌频率就是**6 HZ**，拍的越快，频率就越高。  

> 角频率   

![角频率](./img/Fourier/03.png ':size=WIDTHxHEIGHT')  
角，角度。我在圆心点固定一条杆的一端，使得它向上旋转，这就使得和水平方向有了一个夹角。  
继续旋转会带来角度的变化。**当继续转动旋转一圈后回到原位，就是一次循环了**   
当一秒钟转动了3圈，就是3Hz，3圈转化为弧度制为@6 \pi@, 这就是角频率了，记为@\text{rad/s}@，这里的@\text{rad/s}@是角频率   @\omega@的单位。   
角频率越大，转圈的速度也就越快。     

@@
\omega = 2 \pi f
@@

> 角频率如何与正弦函数产生关联？   

![角频率](./img/Fourier/04.jpg ':size=WIDTHxHEIGHT')  
@\theta@可以用角频率进行表示，即@\text{角速度}  \times \text{时间}@。    

增加一条时间轴t，可以得到一个基于时间的函数    
@@
f(t) = \sin(\omega t)
@@  

![角频率](./img/Fourier/05.jpg ':size=WIDTHxHEIGHT')    

从侧面看，就是正弦波形了，角频率@\omega@越大，则波形越紧密。   

> 频域由来   

傅里叶级数是把一个复杂信号分解成多个正弦信号的叠加，分解出来那么多正弦信号，  
这些正弦信号最明显的特征是什么呢？就是@\text{角频率} \omega@      

这就是频域的由来     
![频域](./img/Fourier/06.png ':size=WIDTHxHEIGHT')     





## 三角函数系正交性 

> 内积的几何和代数性质

平面解析几何向量内积公式      
@@
\mathbf{a} \cdot \mathbf{b} = |\mathbf{a}| |\mathbf{b}|  \cos\theta
@@
当@\theta@角度为@90°@时，两向量互相垂直，@\cos{90} = 0@两向量正交。  


n维空间中的向量内积代数形式   
@@
\mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^{n} a_i b_i
@@

二维空间向量垂直   

@(2,1)*(-1,2) = 2*(-1)+2*1 = 0@

三维空间向量垂直

@(1,2,5)*(1,2,-1) = 1+4-5 = 0@   

@@
\dots
@@

> 函数内积  

函数内积的表现形式是通过在指定区间内，对两个函数的乘积进行积分。即在区间 @[a, b]@ 计算函数 
@f(x)@ 和@g(x)@ 的乘积的积分，定义为它们的内积。     

@@
⟨f, g⟩ = ∫_a^b f(x) g(x) dx
@@

当函数内积为0时，我们就说这两个函数正交。   

> 三角函数系（trigonometric function system）

三角函数系（trigonometric function system）  
通常指的是一个包含一系列三角函数的集合。这些函数用于描述和分析周期性现象或信号。    

@@
\{0, 1, \sin x, \cos x, \sin 2x, \cos 2x, \dots, \sin nx, \cos nx\}
@@

@\sin x@ @\cos x@是基本的周期性函数，它们分别具有周期  它们可以组合成更复杂的周期函数。    


> 三角函数系的正交性

**三角函数系中的不同函数在给定区间上的内积为零，即它们在该区间上是正交的**。
对于三角函数系，其组合形式无非有如下五种，如证明五种组合形式均为正交，则可以推导出三角函数系拥有正交性。     
如下通过牛顿莱布尼兹公式可以简单的求解。   

![组合形式](./img/Fourier/02.png ':size=WIDTHxHEIGHT')  

## 2Π为周期函数傅里叶级数展开  



