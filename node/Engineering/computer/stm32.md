


## 开发环境搭建

> windows下 搭建基于vscode编译器的 stm32交叉编译开发环境搭建  


**交叉编译环境**  

运行编译器的机器（CUP架构） 其使用编译工具得到的可执行程序（CPU架构）不相同，我们称这种编译环境为交叉编译环境。   
因为，例如stm32这种设备，其自身没有算力去安装一个编译环境，进行代码编译。   

在如下这套构建系统中，cmake解码CMakeList.txt，尔后调用make工具Ninja 调用编译器 完成整个工程的编译。  


**下载安装包**

* cmake  

* ninja   
`https://github.com/ninja-build/ninja/releases`

* arm-gnu-toolchain   
`https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads`    
`arm-gnu-toolchain-14.2.rel1-mingw-w64-i686-arm-none-eabi.zip`

* OpenOCD  
`https://gnutoolchains.com/arm-eabi/openocd/`

* llvm   
`https://github.com/llvm/llvm-project`     
`clang+llvm-20.1.2-x86_64-pc-windows-msvc.tar.xz`  


**解压安装包并添加Path**
除了ninja单单一个可执行文件，其余path都是对应模块的bin目录。   
OpenOCD需单独建立一个path变量： `OPENOCD_SCRIPTS = OpenOCD\share\openocd\scripts`  

**vscode插件**  

* `clangd`：语法高亮，代码导航
* `CMake Language Support`：cmake语法高亮，代码补全  
* `Task Buttons`：优化左侧菜单列表
* `Cortex-Debug`：调试时
* `LinkerScript`    


## OpenOCD烧录  

> 检索系统中的 CMSIS-DAP 端口

```py
pip install pyocd
pyocd list
```

> 烧录   

通过CMSIS-DAP调试器，向指定目标芯片 STM32F4 系列烧录文件
```sh
openocd -f interface/cmsis-dap.cfg -f target/stm32f4x.cfg ^
  -c "test_lvgl.elf verify reset exit"
```


