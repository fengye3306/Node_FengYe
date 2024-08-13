# Kolmogorov-Smirnov  

检验是统计学中用于检验数据是否符合某种理论分布（如正态分布）的一种方法。它由两位俄国数学家 Andrey Kolmogorov 和 Nikolai Smirnov 提出了，因此得名“Kolmogorov-Smirnov 检验”。   



## 正态性（Normality）校验

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
#include <boost/math/distributions/normal.hpp>

// 计算数据集的均值
double calculateMean(const std::vector<double>& data) {
    double sum = 0.0;
    for (double value : data) {
        sum += value;
    }
    return sum / data.size();  // 返回平均值
}

// 计算数据集的标准差
double calculateStandardDeviation(const std::vector<double>& data, double mean) {
    double sum = 0.0;
    for (double value : data) {
        sum += std::pow(value - mean, 2);
    }
    return std::sqrt(sum / (data.size() - 1));  // 返回标准差
}

// 根据样本量和显著性水平计算 Kolmogorov-Smirnov 检验的临界值
double calculateCriticalValue(double alpha, size_t n) {
    double c_alpha;
    if (alpha == 0.10) {
        c_alpha = 1.22;
    } else if (alpha == 0.05) {
        c_alpha = 1.36;
    } else if (alpha == 0.01) {
        c_alpha = 1.63;
    } else {
        c_alpha = 1.36; // 默认为 0.05 的显著性水平
    }
    return c_alpha / std::sqrt(n);  // 返回临界值
}

// 执行 Kolmogorov-Smirnov 检验
bool kolmogorovSmirnovTest(const std::vector<double>& data, double alpha) {
    double mean = calculateMean(data);
    double stddev = calculateStandardDeviation(data, mean);
    boost::math::normal_distribution<double> normal_dist(mean, stddev);

    std::vector<double> sorted_data = data;
    std::sort(sorted_data.begin(), sorted_data.end());  // 对数据进行排序

    double n = static_cast<double>(sorted_data.size());
    double d_max = 0.0;

    // 计算 D_max 值
    for (size_t i = 0; i < sorted_data.size(); ++i) {
        double F_i = (i + 1) / n;  // 经验分布函数
        double F_n_i = boost::math::cdf(normal_dist, sorted_data[i]);  // 理论分布函数
        double d = std::fabs(F_i - F_n_i);  // 计算两者的差值
        if (d > d_max) {
            d_max = d;  // 更新最大差值
        }
    }

    double critical_value = calculateCriticalValue(alpha, n);  // 自动计算临界值

    // 判断 D_max 是否小于临界值
    return d_max < critical_value;
}

int main() {

    // 被校验的数据集
    std::vector<double> data = {8.1, 8.0, 7.9, 8.2, 8.0, 7.9, 8.1, 8.0};
    // 显著性水平值
    double alpha = 0.05;  

    if (kolmogorovSmirnovTest(data, alpha)) {
        std::cout << "数据集接近正态分布" << std::endl;  // 数据符合正态分布
    } else {
        std::cout << "数据集不符合正态分布" << std::endl;  // 数据不符合正态分布
    }
    return 0;
}
```
