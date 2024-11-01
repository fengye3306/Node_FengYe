## boost 

> windows

1. 下载boost库文件

`https://github.com/blackberry/Boost`   

2. 双击bootstrap.bat文件，生成b2.exe；  

3. 运行b2.exe编译项目  

```cpp
# 例
./b2 toolset=gcc --prefix=C:/path/to/boost_install link=static runtime-link=shared threading=multi variant=release address-model=64

toolset=gcc：指定使用MinGW（gcc）工具链。
--prefix=C:/path/to/boost_install：指定安装路径，请将其替换为您想要的实际安装路径。
link=static：生成静态库。
runtime-link=shared：使用共享的C++标准库（DLL）。
threading=multi：启用多线程支持。
variant=release：指定生成Release版本。
address-model=64：生成64位库，适用于64位的MinGW编译器（如mingw1120_64）。
```

4. 安装

`./b2 install --prefix=C:/Boost/Install`



