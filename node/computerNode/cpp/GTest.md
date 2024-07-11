*https://github.com/google/googletest*


# 测试套件、测试用例   

*测试用例（Test Case）：* 是指一组测试操作，每个测试用例都应该是自包含和独立的，也就是说它不应该依赖其他测试用例的结果，也不应该影响其他测试用例的运行。     
*测试套件（Test Suite）：* 是一组相关测试用例的集合。你可以把它看作是对一个类或者一个功能模块进行测试的所有测试用例的集合，测试套件的名字通常是被测试的类名或者模块名，这有助于我们在查看测试报告时能快速地知道每个测试用例是用来测试什么的。   

> Example   

TestAddition和TestSubtraction就是测试用例。它们测试了Calculator类的add和subtract方法。         
`CalculatorTest`就是测试套件，表示测试用例之间的相关性。   

```c++
TEST(CalculatorTest, TestAddition) {
    Calculator calculator;
    EXPECT_EQ(calculator.add(2, 2), 4);
}

TEST(CalculatorTest, TestSubtraction) {
    Calculator calculator;
    EXPECT_EQ(calculator.subtract(2, 2), 0);
}
```