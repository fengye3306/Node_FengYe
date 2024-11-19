# 变量名的字符串并映射数据获取  

在开发中，有时需要获取对象或变量名称的字符串形式，以便用于日志记录、数据映射、配置读取等。C++ 提供了一种简洁的方式将变量名转换成字符串，即通过 预处理器的 `#` 操作符。


在 C++ 预处理器中，`#` 操作符称为“字符串化操作符”，它用于将宏参数直接转换为字符串字面量。这可以让我们在代码运行时获取变量名的字符串形式，而不需要手动书写字符串。     

假设我们有一个 QJsonObject 或 std::map 类型的对象，用于存储配置信息。可以通过将变量名转换为字符串来访问与变量名相对应的键值。示例如下：  


```cpp

#include <QJsonObject>
#include <QJsonDocument>
#include <QString>
#include <iostream>

#define TO_STRING(variable) #variable

int main() {
    QJsonObject config;
    config["FontSize"] = 12;

    QString variableName = TO_STRING(FontSize); // 变量名转换为 "FontSize"
    int fontSize = config[variableName].toInt(); // 使用变量名字符串访问 JSON 配置

    std::cout << "Font size is: " << fontSize << std::endl;

    return 0;
}
```
