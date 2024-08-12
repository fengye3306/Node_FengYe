# Kolmogorov-Smirnov  

检验是统计学中用于检验数据是否符合某种理论分布（如正态分布）的一种方法。它由两位俄国数学家 Andrey Kolmogorov 和 Nikolai Smirnov 提出了，因此得名“Kolmogorov-Smirnov 检验”。   



## 正态性（Normality） 
*Kolmogorov-Smirnov实现正态性（Normality）校验*

* **样本量（Sample Size）**   
样本量指的是在统计分析中使用的数据点的数量。例如，如果你测量了一批产品的重量并收集了 100 个样本，那么样本量就是 100。样本量对统计检验结果有重要影响。一般来说，样本量越大，检验结果越可靠，因为较大的样本量能更准确地反映总体特征。

* **显著性水平（Significance Level）**  
显著性水平是用来决定统计检验结果是否“显著”的标准。它表示你愿意接受的错误比例。也就是说，显著性水平决定了你在做判断时容许的错误概率。
例如，在药物效果测试中，你设定了显著性水平来决定实验结果是否可靠。显著性水平就像是你给自己设定的一个“容错范围”，决定了你允许多大的错误发生。显著性水平 @𝛼@ 帮助你决定实验结果是否值得信赖。较低的显著性水平意味着你需要更强的证据才能拒绝零假设，这使得结果更加可信。常见的显著性水平有 0.10、0.05 和 0.01。

* **临界值（Critical Value）**   
临界值是根据显著性水平和样本量计算得出的分界值，用于判断检验结果是否显著。它在统计分布中划定了一个“分界线”，帮助你决定是否拒绝零假设。
临界值的计算依赖于显著性水平和样本量。显著性水平越低，临界值通常越高。样本量越大，临界值也会相应变化。你可以通过统计表格或计算方法确定临界值。



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
