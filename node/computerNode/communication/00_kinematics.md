# GPIB

```msg
-- 本文章使用:NI-488.2协议 --
协议下载地址:https://www.ni.com/zh-cn/support/downloads/drivers/download.ni-488-2.html#544048
```

## 名词解读  

> 设备 （device）   

在 GPIB 系统中，"设备"（device）通常指那些与主控器进行交互的从设备，如仪器、传感器或测试设备。这些设备通常是GPIB总线上的从属设备，负责响应控制器的命令或请求，并发送或接收数据。

> 控制器（board）

控制器（controller）在 GPIB 系统中是负责管理总线和设备的主设备。控制器一般是计算机上安装的 GPIB 接口卡或主板上的通信模块，因此用 brd（board）来表示它。控制器的角色通常是在整个通信过程中控制总线的流程，比如处理设备间的中断、同步数据传输等。  

## 通信异常

> 通信诊断全局值： Ibsta

Ibsta 是 GPIB（通用接口总线）的状态字，它包含了有关 GPIB 及其硬件状态的信息。  
Ibsta 中存储的值是所有传统 NI-488.2 调用（除了 ibfind 和 ibdev）的返回值。你可以检查 Ibsta 中的各个状态位，并利用这些信息来确定应用程序的下一步操作。
为了在每次 NI-488.2 调用之后检查是否出现错误，可以使用 Ibsta 中的 ERR 位。  

```cpp
if (Ibsta() & ERR){
    printf("GPIB error encountered");
}
```

`Ibsta`是一个十六位数，数的每一位都表征了一个具体的异常，当位处于导通状态表征异常触发：  

| 助记符 | 位  | 十六进制 | 类型     | 描述                   |
| ------ | --- | -------- | -------- | ---------------------- |
| **ERR**  | 15  | 8000     | dev, brd | NI-488.2 错误          |
| **TIMO** | 14  | 4000     | dev, brd | 超过时间限制            |
| **END**  | 13  | 2000     | dev, brd | 检测到 END 或 EOS       |
| **SRQI** | 12  | 1000     | brd      | 收到 SRQ 中断           |
| **RQS**  | 11  | 800      | dev      | 设备请求服务            |
| **CMPL** | 8   | 100      | dev, brd | 输入/输出操作完成       |
| **LOK**  | 7   | 80       | brd      | 锁定状态                |
| **REM**  | 6   | 40       | brd      | 远程状态                |
| **CIC**  | 5   | 20       | brd      | 控制器占用状态          |
| **ATN**  | 4   | 10       | brd      | 断言 Attention 信号     |
| **TACS** | 3   | 8        | brd      | 讲话者状态              |
| **LACS** | 2   | 4        | brd      | 听话者状态              |
| **DTAS** | 1   | 2        | brd      | 设备触发状态            |
| **DCAS** | 0   | 1        | brd      | 设备清除状态            |

> 通信诊断全局值： Iberr



> 通信诊断全局值： Ibcnt  



> 通信异常相关接口

* unsigned int **ThreadIbsta** ()    

ThreadIbsta 函数返回当前执行线程的 `Ibsta` 值。




## 轮询查询  

*文档中搜索：Parallel Polling with Traditional NI-488.2 Calls*  


