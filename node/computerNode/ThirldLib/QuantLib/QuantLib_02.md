
### 定价日

*定价日（value date）*

> QuantLib

```cpp
// 创建日期，Quabtlib库中 通过Data类型表示时间
Date data_ValueDate(18, September, 2020);
// 设置定假日
Settings::instance().evaluationDate() = data_ValueDate;
```

QuantLib中,对于定假日的设置在Settings中，其起全局影响。   

> 解读   

"定价日"是一个重要的金融术语，它设定了我们所说的"现在"的具体日期。  
例如，当我们说到"现金流折现"时，这个过程实际上是把未来的收入转化为现在的价值，因为通常情况下，我们认为现在的钱比未来的钱更有价值（也就是所谓的时间价值）。在这个折现过程中，我们需要一个参考日期，以明确所说的"现在"是何时，这个参考日期就是"定价日"。所以，定价日实质上设置了我们进行金融计算时的"现在"日期。     



### Business Day Convention   

*工作日规则（Business Day Convention）*    
当一个重要的金融日期（如付息日或交割日）落在非工作日时，应该如何对这个日期进行调整。   

> QuantLib

QuantLib中使用枚举值`QuantLib::BusinessDayConvention`表述工作日规则。 

* Following：选择给定节假日后的第一个工作日。
* ModifiedFollowing：选择给定节假日后的第一个工作日，除非这一天属于不同的月份，那么就选择节假日前的第一个工作日。
* Preceding：选择给定节假日前的第一个工作日。
* ModifiedPreceding：选择给定节假日前的第一个工作日，除非这一天属于不同的月份，那么就选择节假日后的第一个工作日。
* Unadjusted：不进行调整。
* HalfMonthModifiedFollowing：选择给定节假日后的第一个工作日，除非这一天过了一个月的中间（15号）或者月底，那么就选择节假日前的第一个工作日。
* Nearest：选择最接近给定节假日的工作日。如果前一天和后一天离节假日的距离一样远，那么默认选择后一天。   

```cpp
QuantLib::BusinessDayConvention 
    businessDayConvention = Unadjusted;
```


### Frequency   

*频率 （Frequency）*     
表示在特定时间段内（如日内、日常、周、月、季度或年度）的事件发生的次数。这是一个非常重要的金融术语，因为它可以帮助描述并理解各种金融事件（如利息支付、股息派发、贷款偿还等）的时间安排。    

> QuantLib

库中使用`QuantLib::Frequency`枚举表征频率   

* NoFrequency：无频率，表示不执行。
* Once：仅执行一次，比如零息债券的支付。
* Annual：每年执行一次。
* Semiannual：每半年执行一次。
* EveryFourthMonth：每四个月执行一次。
* Quarterly：每季度执行一次，也就是每三个月。
* Bimonthly：每两个月执行一次。
* Monthly：每月执行一次。
* EveryFourthWeek：每四周执行一次。
* Biweekly：每两周执行一次。
* Weekly：每周执行一次。
* Daily：每日执行一次。
* OtherFrequency：其他未知频率。

### 日期计数规则   

*日期计数规则 Date Count Convention*   


