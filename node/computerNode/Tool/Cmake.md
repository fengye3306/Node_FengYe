# Cmake




## RPATH  

> REPATH字段

```cpp
# 启动REPATH ：CMAKE_INSTALL_RPATH_USE_LINK_PATH
# example
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

# 添加库寻址路径 ：CMAKE_INSTALL_RPATH
# example
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/")
```

> RPATH失效的问题  

1. RPATH添加在add_executable 之前反而生效，之后则不生效，这和官方文档背离
2. 在ubuntu系统下稳定运行的RPATH 在windows上完全不生效

## 设定cpp标准


> 一次标准设定引发的问题

```cpp
'std::filesystem::__cxx11::path' and 'std::filesystem::__cxx11::path')
	|| (__p.has_root_name() && __p.root_name() != root_name()))
note:   'std::filesystem::__cxx11::path' is not derived from 'const std::reverse_iterator<_Iterator>'
这个错误通常是由于使用了不兼容的迭代器类型或不正确的类型比较导致的。具体来说，std::filesystem 中的 path 类型与某些 STL 组件之间的类型不匹配。
```

解决方案当然是启用更高的cpp标准。    
当然，你知道，你早已经启用了cpp17标准。  
```cmake
# 支持c++17标准
target_compile_features(
	${PROJECT_NAME} PRIVATE cxx_std_17
)
```
甚至当你将标准降级为11，或是使用
```cpp
# 设置 C++ 标准为 C++17
set(CMAKE_CXX_STANDARD 17)  
```
都不会再引发这个问题.  
为什么？  


## find_package

`find_package指令用于在已知环境中导入包模块`  
然而，对于包名：是Qt?还是qt?是OpenCV?亦或是opencv?你是否也因为这件事头疼过？     

*nodeeditor* 是一个第三方库，我将其编译为动态链接库，cmake项目欲以find_package将其引入。    
```cmake
find_package(nodeeditor REQUIRED)    
```
error！！！*nodeeditor*库未找到。   
你很沮丧,不知所措。     

find_package的调用其实基于*Config.cmake*文件，*nodeeditor* 库编译后形成的config寻址文件名为：*QtNodesConfig.cmake* ，find_package调用时应该截取`QtNodes`这一字符。   

```cmake
find_package(QtNodes REQUIRED)  
```
此时成功寻址。   
下次需要用到find_package 报错头疼时，不妨去看看config文件前缀。

