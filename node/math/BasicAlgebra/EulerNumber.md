# 欧拉数 e

欧拉数@e@是一个重要的数学常数，定义为一个极限形式。    
当@n@无限趋近于无穷大时，表达式@\left(1 + \frac{1}{n}\right)^n@的极限值就是@e@    
欧拉数@e@的近似值为2.71828,是一个无理数。   

@@
e = \lim_{n \to \infty} \left(1 + \frac{1}{n}\right)^n
@@

> 欧拉数的由来——复利问题

欧拉数@e@最初是在研究复利问题时引入的。在这种背景下，如果我们每年将本金@1@的利率@r@进行@n@次分割  
当@n@趋向于无穷时，本金利息之和将无限趋近于@e^r@。  

```cpp
long double CalculateE(long long numDivisions) {
    // 使用 long long 类型增加分割次数，使用 long double 类型增加精度
    long double increment = 1.0L / numDivisions;
    long double eApproximation = pow(1.0L + increment, numDivisions);
    return eApproximation;
}

int main() {
    long long divisions = 1000000000; // 10亿次分割
    long double e = CalculateE(divisions);

    std::cout.precision(20); // 设置输出精度

    // e: 2.7182820520115602569
    std::cout << "Approximation of e: " << e << std::endl;
    return 0;
}
```

