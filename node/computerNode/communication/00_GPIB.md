# GPIB

本文章使用:NI-488.2协议,协议下载地址:`https://www.ni.com/zh-cn/support/downloads/drivers/download.ni-488-2.html#544048`


## 名词 

> 设备 (device)    
在 GPIB 系统中,"设备"(device)通常指那些与主控器进行交互的从设备,如仪器、传感器或测试设备。这些设备通常是GPIB总线上的从属设备,负责响应控制器的命令或请求,并发送或接收数据。

> 控制器(board)   
控制器(controller)在 GPIB 系统中是负责管理总线和设备的主设备。控制器一般是计算机上安装的 GPIB 接口卡或主板上的通信模块,因此用 brd(board)来表示它。控制器的角色通常是在整个通信过程中控制总线的流程,比如处理设备间的中断、同步数据传输等。  


## 全局状态字

对于访问 NI4882 API 的应用程序,每次 NI-488.2 调用都会更新三个全局函数,以反映您正在使用的设备或板卡的状态。这些全局状态函数包括状态字(Ibsta)、错误函数(Iberr)和计数函数(Ibcnt)。它们包含有关应用程序性能的有用信息。您的应用程序应在每次 NI-488.2 调用后检查这些函数。   

### 通信状态字-Ibsta

Ibsta 是 GPIB(通用接口总线)的状态字,它包含了有关 GPIB 及其硬件状态的信息。  
Ibsta 中存储的值是所有传统 NI-488.2 调用(除了 ibfind 和 ibdev)的返回值。你可以检查 Ibsta 中的各个状态位,并利用这些信息来确定应用程序的下一步操作。
`Ibsta`是一个十六位数,每一位都表征了一个状态字。

`unsigned int **ThreadIbsta** ()`,通过ThreadIbsta 函数返回当前执行线程的 `Ibsta` 值。

| 助记符 | 位  | 十六进制 | 类型     | 描述                   |
| ------ | --- | -------- | -------- | ---------------------- |
| **ERR**  | 15  | 8000     | dev, brd | NI-488.2 错误       |
| **TIMO** | 14  | 4000     | dev, brd | 超过时间限制            |
| **END**  | 13  | 2000     | dev, brd | 检测到 END 或 EOS     |
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

* ERR (错误位): 指示是否发生错误。如果 ERR 位被设置,说明在执行过程中出现了问题,这时需要通过 Iberr() 获取错误代码以确定具体问题。
* TIMO (超时位): 表示操作超时。如果设置了 TIMO 位,说明设备在指定的时间内没有完成操作,这对调试时间相关的问题非常重要。
* END (结束位): 表示检测到结束(END)或结束序列(EOS)。这对于读取数据流时非常重要,能帮助判断数据的完整性。
* SRQI (SRQ 中断位): 表示接收到服务请求(SRQ)。这可以帮助确定设备是否需要进一步的服务。
* RQS (请求服务位): 指示设备请求服务,可以在多设备的环境中使用。
* CMPL (完成位): 表示输入/输出操作已完成,这对于确认操作成功至关重要。
* LOK (锁定状态位): 指示设备是否处于锁定状态,这对于控制访问设备非常重要。
* REM (远程状态位): 表示设备是否处于远程控制状态。
* CIC (控制器占用状态位): 指示当前设备是否为控制器,这对于设备之间的协调至关重要。
* ATN (注意信号位): 表示注意信号是否被断言,这在设备通信中是关键的。
* TACS (讲话者状态位) 和 LACS (听话者状态位): 用于确认哪个设备在进行数据传输。
* DTAS (设备触发状态位) 和 DCAS (设备清除状态位): 用于控制和状态管理。

### 异常状态字-iberr

为了在每次 NI-488.2 调用之后检查是否出现错误,可以使用 Ibsta状态字 中的 ERR 位进行判定是否发生了异常   

```cpp
if (ThreadIbsta() & ERR){
    printf("GPIB error encountered");
}
```

虽然通过`(ThreadIbsta() & ERR)`确定通信异常,但是需要定位到具体异常原因时就应该使用`iberr`状态字     

| **缩写** | **值** | **含义**                    |
|----------|--------|-----------------------------|
| **EDVR** | 0      | 系统错误                     |
| **ECIC** | 1      | 函数要求 GPIB 接口为 CIC    |
| **ENOL** | 2      | GPIB 上没有监听器           |
| **EADR** | 3      | GPIB 接口地址不正确         |
| **EARG** | 4      | 函数调用参数无效             |
| **ESAC** | 5      | GPIB 接口不是所需的系统控制器 |
| **EABO** | 6      | 输入/输出操作被中止(超时) |
| **ENEB** | 7      | 不存在的 GPIB 接口           |
| **EDMA** | 8      | DMA 错误                     |
| **EOIP** | 10     | 异步 I/O 正在进行中          |
| **ECAP** | 11     | 无法进行该操作               |
| **EFSO** | 12     | 文件系统错误                 |
| **EBUS** | 14     | GPIB 总线错误                |
| **ESRQ** | 16     | SRQ 卡在开启状态             |
| **ETAB** | 20     | 表格问题                     |
| **ELCK** | 21     | 接口被锁定                   |
| **EARM** | 22     | ibnotify 回调未能重新准备     |
| **EHDL** | 23     | 输入句柄无效                 |
| **EWIP** | 26     | 指定输入句柄正在等待中        | 
| **ERST** | 27     | 由于接口重置,事件通知被取消  |
| **EPWR** | 28     | 接口失去电源                 |

### IO字符计数-Ibcnt  

`Ibcnt`是计数状态字,它包含关于最近一次 I/O 操作中通过 GPIB 传输的字节数量的信息。
计数函数在每次读取、写入或命令函数后进行更新。此外,在某些错误情况下,Ibcnt 也会在特定的 488.2 风格函数之后更新。
如果读取的数据包含 ASCII 字符,可以使用 Ibcnt 来 NULL 终止字符串,并将其视为其他 ASCII 字符串。例如,可以使用 printf 将结果打印到屏幕: 

> 通过 Ibcnt状态字优化缓冲区读取的有效性

缓冲区很大,而读取的文本输入却很少,如何进行优化？请使用Ibcnt状态字   

```cpp
// 字符串数字,可以存储20个字符,加上一个 NULL 字符 '\0',用来标识字符串的结束
char rdbuf[21];

/*
* ibrd  是一个用于从 GPIB 设备读取数据的函数。
* ud    是一个设备句柄,代表要读取数据的 GPIB 设备。
* rdbuf 是指向读取缓冲区的指针,数据将被存储在这里。
* 20    是要读取的字节数,这里表示最多读取 20 个字节到 rdbuf 中。
*/
ibrd (ud, rdbuf, 20);

if (!(Ibsta() & ERR)){
    /* - Ibcnt() 函数返回最近一次 I/O 操作成功读取的字节数。
    在本例中,它指示从 GPIB 设备实际读取到的数据字节量。
    例如,当 Ibcnt() 返回 15 时,执行 rdbuf[15] = '\0' 
    将在该位置插入 NULL 字符,标志着字符串的结束,确保后续的字符串处理操作能够正确识别读取的数据范围。
    */
    rdbuf[Ibcnt()] = '\0';

    printf ("Read in string: %s\n", rdbuf);
} else {
    // GPIB Error encountered!
}
```

## 488.2接口函数

### ibconfig  

```cpp
unsigned int ibconfig (int ud, int option, int value)  

将选定板卡或设备的配置项更改为指定值,option 可以是任何定义的选项。

INPUT
    ud:         板卡或设备的单元描述符
    option:     选择要更改的软件配置项的参数
    value:      要更改为的指定配置项的值
ONPUT
    函数返回值:  Ibsta状态字
```

> option 可选值  

| **参数名**            | **功能描述**                                                                                               |
|-----------------------|------------------------------------------------------------------------------------------------------------|
| `IbcPAD (0x0001)`      | 主地址 (Primary Address):用于设置 GPIB 设备的主地址。                                                      |
| `IbcSAD (0x0002)`      | 次地址 (Secondary Address):设置 GPIB 设备的次地址。                                                       |
| `IbcTMO (0x0003)`      | 超时值 (Timeout Value):配置设备操作的超时时间。                                                           |
| `IbcEOT (0x0004)`      | 是否在最后一个数据字节发送 EOI (Send EOI with last data byte?):指定是否在发送最后一个数据字节时发送 EOI。   |
| `IbcPPC (0x0005)`      | 并行轮询配置 (Parallel Poll Configure):用于配置并行轮询模式。                                             |
| `IbcREADDR (0x0006)`   | 重复寻址 (Repeat Addressing):允许重复发送寻址命令。                                                       |
| `IbcAUTOPOLL (0x0007)` | 禁用自动串行轮询 (Disable Auto Serial Polling):配置是否禁用自动串行轮询功能。                              |
| `IbcSC (0x000A)`       | 板是否为系统控制器 (Board is System Controller?):设置此板卡是否具有系统控制器功能。                        |
| `IbcSRE (0x000B)`      | GPIB (通用接口总线)通信中 Remote Enable (REN) 线的控制选项。IbcSRE 可以通过设置或清除来控制 REN 线,用于决定设备是否进入远程控制模式。                      |
| `IbcEOSrd (0x000C)`    | 在 EOS 时终止读取 (Terminate reads on EOS):配置当遇到 EOS 字符时是否停止读取数据。                         |
| `IbcEOSwrt (0x000D)`   | 发送 EOI 与 EOS 字符 (Send EOI with EOS character):指定是否在发送 EOS 字符时发送 EOI。                     |
| `IbcEOScmp (0x000E)`   | 使用 7 位或 8 位 EOS 比较 (Use 7 or 8-bit EOS compare):指定在 EOS 比较中使用 7 位还是 8 位数据。           |
| `IbcEOSchar (0x000F)`  | EOS 字符 (The EOS character):设置 EOS(结束符)字符。                                                     |
| `IbcPP2 (0x0010)`      | 使用并行轮询模式 2 (Use Parallel Poll Mode 2):配置设备使用并行轮询模式 2。                                |
| `IbcTIMING (0x0011)`   | 握手时序（Handshake Timing），在数据传输过程中，发送方和接收方通过特定信号进行同步和确认的过程。它确保双方在正确的时机发送和接收数据，以避免数据丢失或冲突。有三挡数值，数值越小传输越快，越不稳定 |
| `IbcDMA (0x0012)`      | 使用 DMA 进行 I/O (Use DMA for I/O):指定是否使用 DMA 进行 I/O 操作。                                      |
| `IbcSendLLO (0x0017)`  | 启用/禁用发送 LLO (Enable/disable the sending of LLO):配置是否启用发送 LLO 命令。                         |
| `IbcSPollTime (0x0018)`| 设置串行轮询的超时值 (Set the timeout value for serial polls):设置串行轮询的超时时间。                     |
| `IbcPPollTime (0x0019)`| 设置并行轮询的时间长度 (Set the parallel poll length period):配置并行轮询的时间长度。                     |
| `IbcEndBitIsNormal (0x001A)`| 从 IBSTA 的 END 位中移除 EOS (Remove EOS from END bit of IBSTA):指定是否从 IBSTA 的 END 位中移除 EOS。|
| `IbcUnAddr (0x001B)`   | 启用/禁用设备未寻址 (Enable/disable device unaddressing):配置设备是否启用未寻址功能。                     |
| `IbcHSCableLength (0x001F)`| 为高速定时指定电缆长度 (Length of cable specified for high speed timing):设置用于高速定时的电缆长度。 |
| `IbcIst (0x0020)`      | 设置 IST 位 (Set the IST bit):配置 IST 位,用于确定是否参与串行轮询。                                      |
| `IbcRsv (0x0021)`      | 设置 RSV 字节 (Set the RSV byte):配置 RSV 字节,通常用于并行轮询操作。                                    |
| `IbcLON (0x0022)`      | 进入仅侦听模式 (Enter listen only mode):配置设备进入仅侦听模式,不发送命令。                             |
| `IbcEOS (0x0025)`      | ibeos 的宏 (Macro for ibeos):此宏用于便捷地配置 EOS 相关参数。                                             |

> 异常

* EARG: option 或 value 无效。ibconfig 板配置参数选项表列出了有效选项。
* ECAP: 驱动程序无法进行请求的更改。
* EDVR: NI-488.2 驱动程序配置不正确或未正确安装。
* EHDL: ud 无效或超出范围。
* ELCK: 由于另一个进程的现有锁定,无法执行请求的操作。
* ENEB: 接口未安装或未正确配置。
* EOIP: 异步 I/O 正在进行中。
* ESAC: 该板不具备系统控制器能力。

### ibask   

```cpp
unsigned int ibask (int ud, int option, int *value) 

ibconfig是用于写入,而ibask用于读取  

INPUT
    ud:         板卡或设备的单元描述符
    option:     选择要更改的软件配置项的参数
    value:      所选配置项的当前值。
ONPUT
    函数返回值:  Ibsta状态字
```

> value 指针应该初始化  

```cpp
/* 在这里我使用了一个空指针
    之后的ThreadIbsta()函数直接报为ERR状态
    同时，全局异常状态字置为 - （EARG 无效的函数参数）
    int_value_pad 被声明为指向 int 的指针，但是没有指向有效的内存位置。
    直接将空指针传递给 ibask 会导致函数无法写入正确的值，这就是为什么会出现 EARG 错误。
*/
int* int_value_pad = nullptr;
ibask(ud,IbcPAD,int_value_pad); 
```


> option 可选值

`ibconfig` 的所有可选值均可于`ibask`状态值调用,除此之外,有一些只读参数只供`option`调用,如下:   
*文档搜索:ibask Board Configuration Parameter Options*  

### 低级设备管理接口

| **函数名**            | **函数功能解释**                                                                                                                                         |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ibfindA`             | 查找并打开一个设备,`udname` 是设备名称。                                                                                                                |
| `ibrdfA`              | 从指定设备读取数据并保存到文件,`filename` 是目标文件名。                                                                                                 |
| `ibwrtfA`             | 将数据从文件写入设备,`filename` 是源文件名。                                                                                                            |
| `ibfindW`             | 查找并打开一个设备(宽字符版本,仅限Windows),`udname` 是设备名称。                                                                                       |
| `ibrdfW`              | 从指定设备读取数据并保存到文件(宽字符版本,仅限Windows),`filename` 是目标文件名。                                                                        |
| `ibwrtfW`             | 将数据从文件写入设备(宽字符版本,仅限Windows),`filename` 是源文件名。                                                                                   |
| `ibask`               | 查询设备的当前状态或配置,`option` 是查询的选项,`v` 用于存储返回值。                                                                                      |
| `ibcac`               | 控制设备的控制器状态,`v` 用于设置是否启用或禁用控制。                                                                                                    |
| `ibclr`               | 发送设备清除(Device Clear)命令来重置设备。                                                                                                              |
| `ibcmd`               | 向设备发送命令字节流,`buf` 是命令数据缓冲区,`cnt` 是字节数。                                                                                            |
| `ibcmda`              | 同`ibcmd`,但使用异步操作来发送命令。                                                                                                                     |
| `ibconfig`            | 设置设备的配置选项,`option` 是配置项,`v` 是要设置的值。                                                                                                 |
| `ibdev`               | 初始化并打开设备连接,参数指定GPIB设备的主地址、次地址、超时、终止方式等。                                                                                 |
| `ibexpert`            | 高级设备控制函数,用于执行特定操作,`option` 用于指定操作类型,`Input` 和 `Output` 用于传递输入和输出数据。                                               |
| `ibgts`               | 启用或禁用GPIB总线触发(Group Trigger)。                                                                                                                 |
| `iblck`               | 锁定设备以防止其他控制器干扰,`v` 是锁定控制,`LockWaitTime` 是等待时间。                                                                                 |
| `iblines`             | 查询当前GPIB总线的信号线状态,`result` 用于存储状态结果。                                                                                                |
| `ibln`                | 查询指定设备是否存在于总线上,返回是否有设备正在监听。                                                                                                    |
| `ibloc`               | 使设备进入本地模式(Local Mode),即退出远程控制模式。                                                                                                     |
| `ibnotify`            | 设置一个回调函数,当特定事件发生时,调用该回调函数。                                                                                                      |
| `ibonl`               | 关闭设备连接或使设备离线。                                                                                                                                |
| `ibpct`               | 通过GPIB总线发送总线清除(Purge Control Transfer)命令。                                                                                                   |
| `ibppc`               | 设置设备的并行轮询配置。                                                                                                                                 |
| `ibrd`                | 从设备读取数据到指定缓冲区,`buf` 是缓冲区,`cnt` 是读取的字节数。                                                                                        |
| `ibrda`               | 同`ibrd`,但使用异步操作来读取数据。                                                                                                                      |
| `ibrpp`               | 读取并行轮询响应数据,存储到`ppr`中。                                                                                                                     |
| `ibrsp`               | 读取设备状态字节,存储到`spr`中。                                                                                                                         |
| `ibsic`               | 发送GPIB系统初始化命令(Send Interface Clear)来初始化总线。                                                                                               |
| `ibstop`              | 停止当前的设备操作。                                                                                                                                      |
| `ibtrg`               | 触发设备执行指定的操作。                                                                                                                                  |
| `ibwait`              | 等待设备发出指定的事件或信号,`mask` 用于指定等待条件。                                                                                                   |
| `ibwrt`               | 向设备发送数据,`buf` 是要发送的数据缓冲区,`cnt` 是发送的字节数。                                                                                        |
| `ibwrta`              | 同`ibwrt`,但使用异步操作发送数据。                                                                                                                       |

### 高级设备管理接口

| 函数名 | 解释 |
|--------|------|
| `AllSpoll` | 执行串行轮询操作,获取指定设备的串行轮询状态。 |
| `DevClear` | 向指定的设备发送设备清除(Device Clear)命令。 |
| `DevClearList` | 向指定的设备列表发送设备清除(Device Clear)命令。 |
| `EnableLocal` | 使设备恢复本地控制模式。 |
| `EnableRemote` | 使设备进入远程控制模式。 |
| `FindLstn` | 查找设备列表中的侦听设备。 |
| `FindRQS` | 查找并返回哪个设备正在请求服务。 |
| `PPoll` | 执行并行轮询操作,返回轮询结果。 |
| `PPollConfig` | 配置指定设备的并行轮询设置。 |
| `PPollUnconfig` | 取消设备列表的并行轮询配置。 |
| `PassControl` | 将总线控制权传递给指定设备。 |
| `RcvRespMsg` | 接收响应消息,并将其存储在缓冲区中。 |
| `ReadStatusByte` | 读取设备的状态字节。 |
| `Receive` | 从指定设备接收数据并存储在缓冲区中。 |
| `ReceiveSetup` | 为设备接收数据做好准备。 |
| `ResetSys` | 重置系统,复位设备列表中的所有设备。 |
| `Send` | 向指定设备发送数据。 |
| `SendCmds` | 向设备发送命令字节。 |
| `SendDataBytes` | 向设备发送数据字节。 |
| `SendIFC` | 发送接口清除(Interface Clear)命令。 |
| `SendLLO` | 发送本地锁定禁止(Local Lockout Disable)命令。 |
| `SendList` | 向指定设备列表发送数据。 |
| `SendSetup` | 为发送数据到设备列表做好准备。 |
| `SetRWLS` | 设置设备的远程写锁(Remote Write Lock)。 |
| `TestSRQ` | 测试服务请求(Service Request)的状态。 |
| `TestSys` | 测试设备列表中的设备。 |
| `Trigger` | 触发设备执行操作。 |
| `TriggerList` | 触发设备列表中的设备执行操作。 |
| `WaitSRQ` | 等待服务请求(Service Request)信号。 |


### IbcSRE   

IbcSRE 是 ibconfig 配置中的一个选项，控制 GPIB Remote Enable (REN) 线的状态：   
板卡(控制器)没有断言(激活)REN线,设备不进入远程模式（设备保持本地模式）。

```msg
IbcSRE
* 输入值：  
    如果 IbcSRE 的值为 非零，REN 线就被断言，设备可以通过远程命令操作。      
    如果 IbcSRE 的值为 零，REN 线被非断言，设备保持在本地操作模式。     
```

在 本地模式 下，设备只能通过手动操作设备上的按钮和旋钮进行控制。  
在 远程模式 下，设备的控制权交给外部设备（比如计算机），无法通过设备上的按钮操作。  
总结：IbcSRE 代表了一个配置选项，控制 GPIB 通信中的 Remote Enable (REN) 线。当你设置 IbcSRE 为非零时，REN 线被断言，设备可以进入远程控制模式；如果 IbcSRE 设置为零，设备保持在本地模式。

### 设备配置为CIC

*主控制器————CIC(Controller-In-Charge)*
CIC就是 当前负责控制 GPIB 总线通信的设备，通常是主机（例如计算机）或者仪器控制器。作为 CIC，设备可以执行以下操作：

* 发送命令来控制总线上的其他设备。
* 指定哪个设备可以讲话（Talker）和哪个设备可以监听（Listener）。
* 控制 GPIB 总线上的数据流动和操作顺序。

当 GPIB 控制器不是 CIC 时，它就不能发出总线控制命令。这时，控制器设备无法指示其他设备发送或接收数据，必须等待当前的 CIC 释放控制权，
或者尝试通过特定操作（如 SendIFC 或 ibsic）成为新的 CIC。  


### 总线设备查询

```cpp
void FindLstn 
    (int boardID, const Addr4882_t *padlist, short *resultlist, size_t limit)  
 
在 GPIB 总线上查找监听设备。

INPUT
    boardID：接口编号，表示当前 GPIB 控制器的编号。
    padlist：一个包含主地址的列表，列表以 NOADDR 作为结束符。
    limit：resultlist 中可存储的最大设备地址数量。
ONPUT
    resultlist：将找到的所有正在监听的设备地址存储在该数组中。
``` 

FindLstn 会依次测试 padlist 中的所有主地址。如果在 padlist 给出的某个主地址上存在设备，
设备的主地址会存储到 resultlist 中。如果没有找到设备，函数会继续测试该主地址的所有从地址，
找到的设备地址同样会存储到 resultlist 中。resultlist 中最多存储 limit 个设备地址。
Ibcnt 保存了 resultlist 中实际存储的地址数量。



> 可能的错误  

* EARG：padlist 中存在无效的主地址，Ibcnt 保存的是 padlist 数组中第一个无效地址的索引。
* EBUS：GPIB总线上没有连接任何设备，可以查看是否是因为接口松动。
* ECIC：接口不是当前的控制者 (CIC)，需参考 SendIFC。
* EDVR：NI-488.2 驱动配置不正确或未正确安装。
* EHDL：boardID 超出范围。
* ELCK：由于被另一个进程锁定，请求的操作无法执行。
* ENEB：接口未安装或配置不正确。
* EOIP：异步 I/O 正在进行中。
* ETAB：GPIB 总线上找到的设备数超过了 limit。



## 轮询查询  
*文档中搜索:Parallel Polling with Traditional NI-488.2 Calls*  


## 直接访问 ni4882.dll 导出函数(C语言)   
*文档中搜索:Directly Accessing the ni4882.dll Exports in C*
