


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

# 例如
openocd -f interface/cmsis-dap.cfg -f target/stm32f4x.cfg -c "program ./test_lvgl.elf verify reset exit"

# 烧录指令变量解析
interface/cmsis-dap.cfg   你所用的调试器接口文件（比如你换 J-Link 就得换成 interface/jlink.cfg）
target/stm32f4x.cfg	      目标芯片系列的配置文件。你用的是 STM32F407，stm32f4x.cfg 是对的
./test_lvgl.elf	          你要烧录的 ELF 文件，路径和文件名可变
"verify reset exit"	      通常不变	可选项，可以根据需要改，比如只想下载不复位就删掉 reset
```

**Q: STM32 的可执行文件都是 ELF 吗？**    
A: 是的，大多数时候是 ELF 格式，特别是用 GCC 工具链（比如 arm-none-eab）
 
**Q: STM32 项目能否像 exe + dll 的方式组织？**     
A: 嵌入式开发也支持“模块化”或“库”的思想，但实现方式不一样。STM32 不支持运行时动态加载       
但是任然可以实现模块化，  
1.把功能打包成静态库（.a），则编译多个模块为 .a 文件，主工程链接这些 .a 
2.FreeRTOS + 动态任务模块（高级玩法）  
不管你有多少个库，**最终都要链接为一个可执行的固件文件（如 .elf、.bin、.hex）**，然后整体烧录。


**Q：STM32开发编译时候会产出bin、elf、hex等类型可被烧录文件，文件区别是什么？**   
`elf:`适合调试（配合 GDB）,但是文件体积较大其**包含调试信息，不作用于生产环境**       
`bin:`只有纯裸的二进制数据，没有地址、没有调试信息,文件小，**适合量产/OTA（通过 bootloader 烧录）**。适合放在外部 Flash，通过 bootloader 加载. 缺点是没有地址信息，烧录时必须手动指定起始地址。    
`hex:`适合生产烧录：可以同时烧录 bootloader、app、config,通常比 .bin 更安全，支持校验。由于是文本格式，**体积略大（比 .bin 稍大）**，其不包含符号信息，**不能调试**    

## GPIO 

> GPIO输出模式  

* **推挽输出（Push-Pull）**：我能输出高也能输出低，啥都自己搞定。     

类似于你有两个开关，一个接电源（高电平），一个接地（低电平）你可以随时打开一个、关闭另一个。所以你可以**主动**输出高电平 或 低电平。    
适合控制LED、蜂鸣器、继电器、芯片复位脚之类的。


* **开漏输出（Open-Drain）**：我只能拉低电平，想要高电平得别人帮我拉起来（上拉）。    

你只有一个开关，只能接地。想要“高电平”？不好意思——你自己搞不定！你得靠别人 “拉起来”（上拉电阻）  
常用于I2C 总线（主从设备共用线），**多个设备一起控制某个信号（比如 N 个芯片都能触发某个中断）**    


* **上拉/下拉电阻（Pull-up / Pull-down）**：默认状态给个电平，防止线悬空。      

当一个引脚是输入状态时，它并不主动输出电平。这时候，如果引脚接了根线但线没电压，它就“懵了”，信号不确定，这叫悬空。  
为了避免这种情况，有两个办法：默认让信号是 高电平，除非你手动拉低，默认让信号是 低电平，除非你手动拉高  

```msg
# 结合起来看看组合意义
推挽输出 + 无上下拉   则标准输出，自己能控制高/低，不需要别人帮忙。   
开漏输出 + 上拉	      常见配置。你放手=高电平，你拉=低电平。   
输入模式 + 上拉	      默认是高电平，如果外部按键按下变低   
输入模式 + 下拉	      默认是低电平，如果外部信号拉高就变高
```

控制LED:推挽输出  
按键检测:输入 + 上拉  
I2C 总线:开漏 + 上拉  
多设备共用信号线:开漏 + 上拉  





