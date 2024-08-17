# 泰勒公式  

## 无穷算法   

泰勒公式源自于16世纪对于无穷算法的研究。  
数学家发现对于任意无穷小数，都可以由@\frac{1}{10^n}@合成：   

@@
\pi = 3.14159\ldots = 3 \times 10^0 + 1 \times \frac{1}{10^1} + 4 \times \frac{1}{10^2} + 1 \times \frac{1}{10^3} + \ldots
@@

通过类比方式，是否任意曲线可以通过@x^n@合成呢？@x^n@拥有良好的数学性能，能极大的简化运算     

将 @\sin(x)@ 展开为 @x@ 的幂级数     
@@
\sin(x) = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \ldots = \sum_{n=0}^{\infty} (-1)^n \frac{x^{2n+1}}{(2n+1)!}
@@

求解@\sin(1)@:   
@@
\sin(1) = 1 - \frac{x^3}{3!} + \frac{x^5}{5!}  \approx 0.84167 
@@ 

随着展开次数趋向于无穷，这个多项式就可以在整个实数轴上与@\sin(x)@重合。     

![一次展开](./img/Taylor/01.png ':size=WIDTHxHEIGHT')
![三次展开](./img/Taylor/02.png ':size=WIDTHxHEIGHT')
![九次展开](./img/Taylor/03.png ':size=WIDTHxHEIGHT')


## 幂级数（Power Series）

幂级数（Power Series）是一个无穷级数，其每一项都是变量的某个非负整数次幂的乘积。幂级数通常用于表示函数，它的数学定义如下：   

@@
\sum_{n=0}^{\infty} a_n (x - x_0)^n = a_0 + a_1(x - x_0) + a_2(x - x_0)^2 + a_3(x - x_0)^3 + \ldots
@@

* @a_n@是常数项系数，被称为指数的系数。
* @x@是变量。
* @x_0@是幂级数的中心（常常为0），决定了级数相对于@x@的展开位置。


## 泰勒公式推导  

对一个**解析函数 @f(x)@** 进行泰勒级数展开，类似于对函数 @\sin{x}@ 使用泰勒级数展开的问题。这一过程相当于将幂级数的形式转换为函数 @f(x)@ 的近似表达。    
**设 @f(x)@ 在点 @x = x_0@ 处具有 @n@ 阶导数，我们的目标是确定一个关于 @(x - x_0)@ 的幂级数 @p_n(x)@，使得 @f(x) - p_n(x)@ 是 @(x - x_0)^{n}@ 的高阶无穷小。**     
说人话就是：我要找到这个幂指数，使得它和这个@f(x)@非常的接近，定义为使得在@X_0@处的函数值、直到n阶导完全相等，每多一阶导数相等即表示你捕捉的细节越多，你雕刻的越像。   

1. 幂级数的原式  

@@
\sum_{n=0}^{\infty} a_n (x - x_0)^n = a_0 + a_1(x - x_0) + a_2(x - x_0)^2 + a_3(x - x_0)^3 + \ldots
@@

2. 一阶导数   
@@
\frac{d}{dx} \left( \sum_{n=0}^{\infty} a_n (x - x_0)^n \right) = \sum_{n=1}^{\infty} a_n \cdot n \cdot (x - x_0)^{n-1} = a_1 + 2a_2(x - x_0) + 3a_3(x - x_0)^2 + \ldots
@@

3. 二阶导数   

@@
\frac{d^2}{dx^2} \left( \sum_{n=0}^{\infty} a_n (x - x_0)^n \right) = \sum_{n=2}^{\infty} a_n \cdot n \cdot (n-1) \cdot (x - x_0)^{n-2} = 2a_2 + 6a_3(x - x_0) + 12a_4(x - x_0)^2 + \ldots
@@

4. 三阶导数   

@@
\frac{d^3}{dx^3} \left( \sum_{n=0}^{\infty} a_n (x - x_0)^n \right) = \sum_{n=3}^{\infty} a_n \cdot n \cdot (n-1) \cdot (n-2) \cdot (x - x_0)^{n-3} = 6a_3 + 24a_4(x - x_0) + \ldots
@@

5. 四阶导数   

@@
\frac{d^4}{dx^4} \left( \sum_{n=0}^{\infty} a_n (x - x_0)^n \right) = \sum_{n=4}^{\infty} a_n \cdot n \cdot (n-1) \cdot (n-2) \cdot (n-3) \cdot (x - x_0)^{n-4} = 24a_4 + \ldots
@@

> 系数@a_n@的推导    


要找到系数 @a_n@的值，需要利用导数在@x=x_0@处的值。  
将幂级数进行@n@此导数，并在@x=x_0@处求值：   

@@
f^{(n)}(x) = \frac{d^n}{dx^n} \left( \sum_{n=0}^{\infty} a_n (x - x_0)^n \right) = n! \cdot a_n + \text{higher terms}
@@  

在@x=x_0@处：   

@@
f^{(n)}(x_0) = n! \cdot a_n
@@  

则系数@a_n@为：  

@@
a_n = \frac{f^{(n)}(x_0)}{n!}
@@  

> 综上推导出标准泰勒公式   

基于上面的推导，函数@f(x)@在@x_0@处的泰勒展开式为：    
@@
p_n(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(x_0)}{n!} (x - x_0)^n = f(x_0) + \frac{f' \quad (x_0)}{1!}(x - x_0) + \frac{f'' \quad (x_0)}{2!}(x - x_0)^2 + \frac{f''' \quad (x_0)}{3!}(x - x_0)^3 + \ldots + \frac{f^{(n)} \quad (x_0)}{n!} (x - x_0)^n 
@@  

@@
f(x) = p_n(x) + \text{高阶无穷小误差（皮亚诺余项）}
@@  


> 证明@f(x)-p_n(x)@是高阶无穷小    

**高阶无穷小定义：**
当@x@接近@x_0@时，@f(x) - p_n(x)@的大小比@(x - x_0)^{n}@要小得多。   

**即证明**    
@@
\lim_{x \to x_0} \frac{f(x) - p_n(x)}{(x - x_0)^{n}} = 0
@@  

使用洛必达法则的条件是：   

1. 使用洛必达法则的前提条件是分子和分母在某个点上都必须是可导的。  
@f(x)@ 和 @p_n(x)@ 是多项式，可导性很好。

2. 使用洛必达法则的前提条件是 @\frac{\text{分子}}{\text{分母}}@ 是 @\frac{0}{0}@ 型或是 @\frac{\infty}{\infty}@ 型。   
设定中 @p_n(x)@ 是 @f(x)@ 的泰勒多项式，因此当 @x@ 无限趋近于 @x_0@ 时 @f(x) - p_n(x) = 0@，   
因此，这里是典型的 @\frac{0}{0}@ 型。

可以使用洛必达法则：  

@@
\lim_{x \to x_0} \frac{f(x) - p_n(x)}{(x - x_0)^{n}} = \lim_{x \to x_0} \frac{f'(x) - p_n'(x)}{n(x - x_0)^{n-1}}
@@

上下同时求导行为可一直持续，直到

@@
\lim_{x \to x_0} \frac{f^{(n-1)}(x) - p_n^{(n-1)}(x)}{ n!(x - x_0)}
@@

分子就不能再洛了。   

为什么？因为我们只可以知道@f(x)@和@p_n@在@x=x_0@处n阶可导，却绝对推不出@x@趋向于@x_0@的过程中n阶可导    

@@
\lim_{x \to x_0} \frac{(f^{(n-1)}(x)-f^{(n-1)}(x_0)) - ( p_n^{(n-1)}(x) -p_n^{(n-1)}(x_0))}{ n!(x - x_0)} = 
@@

拆解定义式     

@@
\lim_{x \to x_0} \frac{1}{n!} (\frac{(f^{(n-1)}(x)-f^{(n-1)}(x_0))}{(x - x_0)} - \frac{( p_n^{(n-1)}(x) -p_n^{(n-1)}(x_0))}{ (x - x_0)})
@@

可以得到

@@
\lim_{x \to x_0} \frac{1}{n!} (\frac{(f^{(n)}(x_0) - \frac{(p_n^{(n)}(x_0))
@@

根据定义可求证高阶无穷小。    



