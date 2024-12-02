
## 系统解析   

### wafer

芯片的生产制造以客户为中心，下图是芯片生产制造简化流程。   

![芯片生产制造简化流程](./img/03_ServiceGuide/01.png ':size=300')   



| 概念        | 类比为计算机编程中的元素    | 解释                                                                                                                                 |
|-------------|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Die**     | **类（Class）**             | **Die** 是集成电路中的核心部分，  
类似于面向对象编程中的类，定义了整体功能的框架。  
一个 **Die** 可以包含多个 **Cell**，  
类似于类中包含多个方法或属性。 |
| **Cell**    | **类的属性（Attributes）或方法（Methods）** | 每个 **Cell** 实现了某个具体功能（如逻辑运算或触发器），  
类似于类中的方法。  
它也可以看作类的属性，代表了 **Die** 的一些特定功能或特点。 |
| **芯片（Chip）** | **对象（Object）**         | **Chip** 是封装好的 **Die**，  
类似于面向对象编程中的实例（对象）。  
它通过封装、连接引脚和封装材料，  
使得芯片可以与外部电路连接并发挥作用。 |



> **wafer**：  

是集成电路制造过程中的基础材料，通常由高纯度的硅构成。硅是从石英砂（SiO₂）中提炼得到的，其实就是那些最不起眼的沙子，不过不是随便抓一把沙子就可以做原料的，一定要精挑细选，从中提取出最最纯净的硅原料才行。  
其提炼过程包括将石英砂经过高温溶化还原生成单晶硅，然后从高温容器中采用旋转拉伸的方式将硅原料取出，此时一个圆柱体的硅锭就产生了。  接下来，这些硅晶棒被切割成薄片，这些薄片即为**元晶圆**，是芯片制造的前驱材料。    
再经过光刻胶(Photo Resist)，溶解光刻胶、蚀刻、离子注入、电镀、铜层生长最终得到晶圆，如下图所示：   
目前，行业中所提及的“6英寸”、“12英寸”和“18英寸”晶圆，实际上是晶圆直径的估算值。其实际直径分别为150mm、300mm和450mm。为了便于命名和沟通，通常使用英寸单位表示晶圆的尺寸，其中12英寸晶圆的实际直径约为305mm。国内的晶圆生产线以8英寸（约200mm）和12英寸（约300mm）晶圆为主。   

> **Die**   

Die（芯片单元，也叫**裸芯片**）是集成电路制造过程中，从硅晶圆（Wafer）上通过光刻和掺杂工艺形成的基本电路单元。  
也可以说，一个wafer通过基于电气特性的探针测试，再用精确控制的切片机将一个个小格切开，而这个小方格就称为Die，一个晶圆Wafer上每个小方块的设计内容都是一样的，全是同一个Die的重复单元。   
在光刻工艺中，经过一系列的曝光、显影、刻蚀等步骤，将电路图案转移到硅晶圆表面，随后通过掺杂（添加杂质）过程改变硅晶体的电学性质，从而在晶圆上形成多个电路单元，通常呈点状或网状分布，这些电路单元被称为Die。    
每个硅晶圆上通常包含大量的Die，这些Die在制造过程中会进行Pin Test（针法测试）以验证其电学性能。Pin Test是通过在晶圆上使用测试针接触每个Die的测试点，测量其电气特性，如电压、电流等，确保每个Die符合设计规格。由于晶圆上通常包含大量的Die，Pin Test的过程相对复杂且需要高度自动化。  
为了提高生产效率和降低成本，通常采用批量生产相同规格和型号的芯片。生产的芯片数量越大，单位成本相对较低，这也是主流芯片制造设备成本较低的重要原因之一。   
将测试合格的die切割下来，做封装，如下如图中所示：   
![切割下来做封装](./img/03_ServiceGuide/02.png ':size=300')   

>  **Chip**  

Chip（封装后的芯片），Chip ==  the packaged die + lead frame + epoxy    

Lead Frame 是一种金属框架，它的主要作用是为封装提供支撑，并连接裸芯片（Die）与外部电路。这个框架通常包括多根金属引脚（leads），这些引脚是用来连接芯片与外部设备或电路的。   
简而言之，Lead Frame 是支持引脚的金属结构，它本身提供的是连接和支撑作用，而引脚（leads） 是 Lead Frame 上的部分，负责实现电气连接。     
Epoxy 一种热固性树脂材料，用于芯片封装中，它的作用是 固定裸芯片、保护芯片、提供电气绝缘、提高机械强度、协助散热。   

> **Cell**  

Cell是电路设计的标准单元，由晶体管和连线结构组成的，具有最基本的布尔逻辑功能或者触发功能。  
Cell 是一种自包含的电路单元，通常实现一个特定的功能。这些功能可以是逻辑功能（例如与门、或门、非门）或者触发器功能（例如D触发器、T触发器等）。每个 Cell 都是一个在特定设计中可以反复使用的单元，具有标准化的尺寸、输入输出端口、逻辑功能和时序特性。     

根据功能的不同，Cell 可以分为以下几类：   

* 逻辑门单元（Logic Gate Cells）：实现基本的布尔逻辑运算，例如与门（AND）、或门（OR）、非门（NOT）等。
例如：AND、OR、NOT、NAND、NOR、XOR 等。    
* 触发器单元（Flip-Flop/Trigger Cells）：用于存储和控制信号的状态，主要用于时序电路。
例如：D触发器、T触发器、JK触发器、RS触发器等。    
* 算术运算单元（Arithmetic Cells）：用于实现加法、减法等算术运算。
例如：加法器（Adder）、乘法器（Multiplier）等。   
* 时钟生成和控制单元（Clock Cells）：用于时钟信号的生成、分配和控制。
例如：时钟缓冲器、时钟分配单元等。
* IO单元（Input/Output Cells）：用于与外部电路或者其他模块连接。
例如：输入缓冲器（Input Buffer）、输出缓冲器（Output Buffer）等。



## 指令表

### 文档目录 

1. AZoom 
2. BnR_KernelDriver 
3. 通用命令 (CommonCommands) 
4. 低温工具 (CryogenicTool) 
5. 内核驱动 (KernelDriver) 
6. 内核设置 (KernelSetup) 
7. 加载器 (Loader) 
8. 消息服务器 (MsgServer) 
9. 通知 (Notifications)
10. 脚本功能 (Scripting) 
11. 频谱分析 (Spectrum) 
12. 子模块窗口 (SubdieWindow) 
13. 表视图 (TableView) 
14. 测试器 (Tester)
15. 工具栏 (Toolbar) 
16. TTL 
17. 晶圆地图 (WaferMap)
18. Z 轴分析 (ZProfiling)
19. 枚举 (Enumerations) 

### 1.AZoom

1. AZoom设置对话框（AZoomSetupDialog）
2. 关闭AZoom（CloseAZoom）
3. 获取AZoom镜头（GetAZoomLens）
4. 移动AZoom焦点（MoveAZoomFocus）
5. 移动AZoom速度（MoveAZoomVelocity）
6. 获取变焦级别（OCGetZoomLevel）
7. 开启光源（OCLightOn）
8. 设置光源值（OCLightVal）
9. 设置变焦级别（OCSetZoomLevel）
10. 读取AZoom焦点（ReadAZoomFocus）
11. 选择AZoom镜头（SelectAZoomLens）
12. 设置AZoom光源（SetAZoomLight）
13. 停止AZoom（StopAZoom）


* **焦（Focal Length）** 它定义了镜头聚焦的能力。具体来说，它是从镜头的光学中心到成像平面（通常是相机传感器或胶片）的距离，单位通常为毫米（mm）。   
* **变焦因子（Zoom Factor）**通常指的是镜头的变焦能力，表示镜头从最小变焦到最大变焦时的放大倍率。在光学设备中，它量化了镜头可以调节的焦距范围。例如，如果一个镜头的变焦因子是4x，意味着镜头的焦距可以从最小焦距扩展到最大焦距的四倍。   



### 2.BnR_KernelDriver

> BnR_KernelDriver

1. 系统（System）
2. BnR_CreateSdmSystemDump（创建Sdm系统转储）
3. BnR_DoInternalTask（执行内部任务）
4. BnR_EchoData（回显数据）
5. BnR_GetAnalogIO（获取模拟输入输出）
6. BnR_GetControllerData（获取控制器数据）
7. BnR_GetControllerType（获取控制器类型）
8. BnR_GetDataIterator（获取数据迭代器）
9. BnR_GetInput（获取输入）
10. BnR_GetOutput（获取输出）
11. BnR_GetStartupStatus（获取启动状态）
12. BnR_GetStationType（获取站点类型）
13. BnR_ReadMessage（读取消息）
14. BnR_ReportKernelVersion（报告内核版本）
15. BnR_ResetController（重置控制器）
16. BnR_SetAnalogOutput（设置模拟输出）
17. BnR_SetOutput（设置输出）
18. BnR_WriteMessage（写入消息）

> 运动（Movement）  

1. BnR_GetAxisState（获取轴状态）
2. BnR_GetAxisStatus（获取轴的状态）
3. BnR_GetInternalAxisInfo（获取内部轴信息）
4. BnR_GetPosition（获取位置）
5. BnR_GetQuietMode（获取静音模式）
6. BnR_GetTraceData（获取轨迹数据）
7. BnR_GetWiringTesterData（获取接线测试数据）
8. BnR_InitAxis（初始化轴）
9. BnR_Move（移动）
10. BnR_MoveAxis（移动轴）
11. BnR_MoveT（T轴移动）
12. BnR_MoveZ（Z轴移动）
13. BnR_MoveZCombined（组合Z轴移动）
14. BnR_ScanMoveZ（扫描Z轴移动）
15. BnR_SearchEdgeSensor（搜索边缘传感器）
16. BnR_SetQuietMode（设置静音模式）
17. BnR_SetTraceMode（设置轨迹模式）
18. BnR_StepMove（步进移动）
19. BnR_StopAxis（停止轴）
20. BnR_SyncMove（同步移动）

> 服务（Service）

1. BnR_DiagClearAxisErrors（诊断清除轴错误）
2. BnR_DiagGetData（诊断获取数据）
3. BnR_DiagMoveAxis（诊断移动轴）
4. BnR_DiagSetAxis（诊断设置轴）
5. BnR_DiagSetParams（诊断设置参数）
6. BnR_DiagStopAxis（诊断停止轴）
7. BnR_GetDatum（获取基准点）
8. BnR_GetNextDatum（获取下一个基准点）
9. BnR_ReadAxisModuleRegister（读取轴模块寄存器）
10. BnR_SetDatum（设置基准点）
11. BnR_WriteAxisModuleRegister（写入轴模块寄存器）

### 3.通用命令 CommonCommands  

1. WinCal（WinCal）
1. WinCalAutoCal（自动校准）
2. WinCalAutoCalNoValidation（无验证自动校准）
3. WinCalCheckAutoRFStability（检查自动射频稳定性）
4. WinCalCloseRFStabilityReport（关闭射频稳定性报告）
5. WinCalExecuteCommand（执行命令）
6. WinCalGetCompleteEnabledIssList（获取已启用ISS列表）
7. WinCalGetIssForAuxSite（获取附加站点的ISS）
8. WinCalGetNameAndVersion（获取名称和版本）
9. WinCalGetNumMonitoringPorts（获取监控端口数量）
10. WinCalGetNumPortsAndProbes（获取端口和探针数量）
11. WinCalGetNumRepeatabilityPorts（获取重复性端口数量）
12. WinCalGetNumValidationPorts（获取验证端口数量）
13. WinCalGetProbeInfoForPort（获取端口探针信息）
14. WinCalGetReferenceStructureInfo（获取参考结构信息）
15. WinCalGetValidationSetup（获取验证设置）
16. WinCalHideAllWindows（隐藏所有窗口）
17. WinCalMeasureMonitorReference（测量监视参考）
18. WinCalMonitorNoMove（监视无移动）
19. WinCalMoveToIssRef（移动到ISS参考位置）
20. WinCalRecordIssRefAtCurrentLoc（在当前位置记录ISS参考）
21. WinCalSetNumMonitoringPorts（设置监控端口数量）
22. WinCalSetNumRepeatabilityPorts（设置重复性端口数量）
23. WinCalSetNumValidationPorts（设置验证端口数量）
24. WinCalSystemSetupHasUnappliedChanges（系统设置有未应用的更改）
25. WinCalUseNextGoodGroupOnIss（在ISS上使用下一个良好的组）
26. WinCalValidate（验证）
27. WinCalValidateAdvanced（高级验证）
28. WinCalVerifyIssRefLocAtHome（验证ISS参考位置是否在家位置）
29. CCMoveAuxSite（移动附加站点）
30. CCReadCurrentLens（读取当前镜头）
31. CCSelectLens（选择镜头）
32. CloseSplashScreen（关闭启动画面）
33. DisplayMessage（显示消息）
34. ExecuteCleaningSequence（执行清洁序列）
35. GetAlignmentMode（获取对准模式）
36. GetLastFourProjects（获取最近四个项目）
37. GetLicenseInfo（获取许可证信息）
38. GetLoginData（获取登录数据）
39. GetMachineState（获取机器状态）
40. GetProjectFile（获取项目文件）
41. GetScopeWorkingStage（获取工作台状态）
42. GetSoftwarePath（获取软件路径）
43. GetStatus（获取状态）
44. IsAppRegistered（应用程序是否注册）
45. LicensingDialog（许可对话框）
46. LoadScopeFenceConfiguration（加载工作台围栏配置）
47. LoginDialog（登录对话框）
48. NucleusInitChuck（初始化夹具）
49. OpenProject（打开项目）
50. OpenProjectDialog（打开项目对话框）
51. ReportSoftwareVersion（报告软件版本）
52. SaveProject（保存项目）
53. SaveProjectAsDialog（另存项目对话框）
54. SetAlignmentMode（设置对准模式）
55. SetScopeWorkingStage（设置工作台状态）
56. ShowAboutDialog（显示关于对话框）
57. ShowSetupDialog（显示设置对话框）
58. ShowSplashScreen（显示启动画面）
59. ShutdownVeloxWithSave（保存并关闭Velox）  

### 4.低温工具（CryogenicTool）

1. CryoCommand（低温命令）
2. CryoMoveBBPark（低温移动车床停靠）
3. CryoMoveBBWork（低温移动车床工作）
4. CryoMoveScopePark（低温移显微镜停靠）
5. CryoMoveScopeWork（低温移显微镜工作）
6. CryoMoveShutter（低温移光阑）
7. CryoReadPressure（读取低温压力）
8. CryoReadState（读取低温状态）
9. CryoReadTemperature（读取低温温度）
10. CryoSetTemperature（设置低温温度）
11. CryoStartRefill（开始低温加注）
12. CryoStopRefill（停止低温加注）

### 5.核心驱动（KernelDriver）   

> 1_Alignment（对准）

   1. AlignCardTheta（对准卡片θ轴）
   2. AlignChuckTheta（对准夹具θ轴）
   3. AlignProbeTheta（对准探针θ轴）
   4. AlignScopeTheta（对准显微镜θ轴）
   5. ReadCardTheta（读取卡片θ轴）
   6. ReadChuckTheta（读取夹具θ轴）
   7. ReadProbeTheta（读取探针θ轴）
   8. ReadScopeTheta（读取显微镜θ轴）

> 2_AUX Sites（辅助站点）

   1. CleanProbeTip（清洁探针尖端）
   2. GetAuxSiteCount（获取辅助站点数量）
   3. GetAuxSiteName（获取辅助站点名称）
   4. GetCleaningParams（获取清洁参数）
   5. MoveAuxSite（移动辅助站点）
   6. ReadAuxHeights（读取辅助站点高度）
   7. ReadAuxIndex（读取辅助站点索引）
   8. ReadAuxPosition（读取辅助站点位置）
   9. ReadAuxStatus（读取辅助站点状态）
   10. ResetCleaningPosition（重置清洁位置）
   11. SetAuxHeight（设置辅助站点高度）
   12. SetAuxHome（设置辅助站点归位）
   13. SetAuxIndex（设置辅助站点索引）
   14. SetAuxMode（设置辅助站点模式）
   15. SetAuxSiteCount（设置辅助站点数量）
   16. SetAuxSiteName（设置辅助站点名称）
   17. SetAuxSiteType（设置辅助站点类型）
   18. SetAuxThetaHome（设置辅助站点θ轴归位）
   19. SetCleaningParams（设置清洁参数）
   20. UpdateAuxSitePositions（更新辅助站点位置）

> 3_Chuck（夹具）

   1. AttachAmbientWafer（附加环境晶圆）
   2. ClearChuckTablePoint（清除夹具台点）
   3. GetChuckTableID（获取夹具台ID）
   4. GetPerformanceMode（获取性能模式）
   5. InitChuck（初始化夹具）
   6. MoveChuck（移动夹具）
   7. MoveChuckAlign（移动夹具对准）
   8. MoveChuckAsync（异步移动夹具）
   9. MoveChuckContact（移动夹具接触）
   10. MoveChuckIndex（移动夹具索引）
   11. MoveChuckLift（移动夹具升降）
   12. MoveChuckLoad（移动夹具加载）
   13. MoveChuckSeparation（移动夹具分离）
   14. MoveChuckSubsite（移动夹具子站）
   15. MoveChuckTablePoint（移动夹具台点）
   16. MoveChuckTransfer（移动夹具转移）
   17. MoveChuckVelocity（移动夹具速度）
   18. MoveChuckZ（移动夹具Z轴）
   19. MoveChuckZSafe（安全移动夹具Z轴）
   20. MoveCoolDownPosition（移动夹具冷却位置）
   21. ReadChuckHeights（读取夹具高度）
   22. ReadChuckIndex（读取夹具索引）
   23. ReadChuckPosition（读取夹具位置）
   24. ReadChuckStatus（读取夹具状态）
   25. ReadChuckTablePoint（读取夹具台点）
   26. ReadChuckThermoScale（读取夹具热标度）
   27. ReadChuckThermoValue（读取夹具热值）
   28. ScanChuckZ（扫描夹具Z轴）
   29. SearchChuckContact（搜索夹具接触）
   30. SetChuckHeight（设置夹具高度）
   31. SetChuckHome（设置夹具归位）
   32. SetChuckIndex（设置夹具索引）
   33. SetChuckMode（设置夹具模式）
   34. SetChuckTablePoint（设置夹具台点）
   35. SetChuckThermoScale（设置夹具热标度）
   36. SetChuckThermoValue（设置夹具热值）
   37. SetPerformanceMode（设置性能模式）
   38. StopChuckMovement（停止夹具移动）

> 4_Notifications（通知）

   1. RegisterNotification（注册通知）

> 5_Offset（偏移）

   1. EnableOffset（启用偏移）
   2. GetOffsetInfo（获取偏移信息）
   3. SetOffset（设置偏移）
   4. SetSwitchPosition（设置开关位置）

> 6_System（系统）

   1. ActivateChuckVacuum（激活夹具真空）
   2. DockChuckCamera（对接夹具相机）
   3. EchoData（回显数据）
   4. EnableEdgeSensor（启用边缘传感器）
   5. EnableMotorQuiet（启用电机静音）
   6. GetAxisReverse（获取轴反转状态）
   7. GetBackSideMode（获取背面模式）
   8. GetControllerInfo（获取控制器信息）
   9. GetDarkMode（获取暗模式）
   10. GetManualMode（获取手动模式）
   11. GetMTransSWLimits（获取MTrans软件限制）
   12. GetNanoChamberState（获取纳米室状态）
   13. GetSoftwareStop（获取软件停止状态）
   14. GetStageLock（获取舞台锁定状态）
   15. InkDevice（初始化设备）
   16. MoveZCombinedGetStatus（获取Z轴联合状态）
   17. MoveZCombinedSetStatus（设置Z轴联合状态）
   18. ProbeStationVerify（验证探针站）
   19. ReadCBoxCurrSpeed（读取CBox当前速度）
   20. ReadCBoxPosMonConf（读取CBox位置监测配置）
   21. ReadCBoxStage（读取CBox舞台状态）
   22. ReadCompensationStatus（读取补偿状态）
   23. ReadContactCount（读取接触计数）
   24. ReadCurrentLens（读取当前镜头）
   25. ReadJoystickSpeeds（读取摇杆速度）
   26. ReadJoystickSpeedsCycle（读取摇杆速度周期）
   27. ReadManualPlatenState（读取手动平板状态）
   28. ReadProberStatus（读取探针状态）
   29. ReadProbeSetup（读取探针设置）
   30. ReadSensor（读取传感器数据）
   31. ReadSystemStatus（读取系统状态）
   32. ReadTurretStatus（读取炮塔状态）
   33. ReadTypedSensor（读取指定类型传感器）
   34. ReadWaferStatus（读取晶圆状态）
   35. ReportKernelVersion（报告内核版本）
   36. ResetContactCount（重置接触计数）
   37. ResetNetworkPort（重置网络端口）
   38. SelectLens（选择镜头）
   39. SendAUCSCommand（发送AUCS命令）
   40. SetBackSideMode（设置背面模式）
   41. SetBeaconStatus（设置信标状态）
   42. SetCameraCool（设置相机冷却）
   43. SetCBoxCurrSpeed（设置CBox当前速度）
   44. SetCBoxPosMonConf（设置CBox位置监测配置）
   45. SetCBoxStage（设置CBox舞台状态）
   46. SetChuckVacuum（设置夹具真空）
   47. SetCompensationStatus（设置补偿状态）
   48. SetDarkMode（设置暗模式）
   49. SetExternalMode（设置外部模式）
   50. SetJoystickSpeeds（设置摇杆速度）
   51. SetJoystickSpeedsCycle（设置摇杆速度周期）
   52. SetLoaderGate（设置装载门）
   53. SetManualMode（设置手动模式）
   54. SetMicroLight（设置微光模式）
   55. SetMTransSWLimits（设置MTrans软件限制）
   56. SetNanoChamberState（设置纳米室状态）
   57. SetOutput（设置输出）
   58. SetSoftwareStop（设置软件停止状态）
   59. SetStageLock（设置舞台锁定状态）
   60. SetTypedOutput（设置指定类型输出）
   61. StopAllMovements（停止所有移动）

> 6.7 Scope（显微镜）

1. ClearScopeTablePoint（清除显微镜台点）
2. GetScopeTable（获取显微镜台）
3. InitScope（初始化显微镜）
4. MoveScope（移动显微镜）
5. MoveScopeAlign（移动显微镜对准）
6. MoveScopeAsync（异步移动显微镜）
7. MoveScopeFocus（移动显微镜焦点）
8. MoveScopeIndex（移动显微镜索引）
9. MoveScopeLift（移动显微镜升降）
10. MoveScopeSeparation（移动显微镜分离）
11. MoveScopeSilo（移动显微镜筒仓）
12. MoveScopeTablePoint（移动显微镜台点）
13. MoveScopeVelocity（移动显微镜速度）
14. MoveScopeZ（移动显微镜Z轴）
15. ReadScopeHeights（读取显微镜高度）
16. ReadScopeIndex（读取显微镜索引）
17. ReadScopePosition（读取显微镜位置）
18. ReadScopeSilo（读取显微镜筒仓）
19. ReadScopeSiloCount（读取显微镜筒仓数量）
20. ReadScopeStatus（读取显微镜状态）
21. ReadScopeTablePoint（读取显微镜台点）
22. SetScopeHeight（设置显微镜高度）
23. SetScopeHome（设置显微镜归位）
24. SetScopeIndex（设置显微镜索引）
25. SetScopeMode（设置显微镜模式）
26. SetScopeSiloReference（设置显微镜筒仓参考）
27. SetScopeTablePoint（设置显微镜台点）
28. StopScopeMovement（停止显微镜移动）

> 6.8 Service（服务）

1. LoadMEAFile（加载MEA文件）
2. ReadMatrixValues（读取矩阵值）
3. ReadMEAStatus（读取MEA状态）
4. ReadStageLocations（读取舞台位置）
5. ResetCBox（重置CBox）
6. ResetProber（重置探针）
7. SetMatrixValues（设置矩阵值）
8. SetOperationalMode（设置操作模式）

> 6.9 Setup（设置）

1. GetDataIterator（获取数据迭代器）
2. GetDatum（获取数据项）
3. GetNextDatum（获取下一个数据项）
4. SetDatum（设置数据项）
5. SetRecoveryDatum（设置恢复数据项）

> 6.10 SWFence（软件围栏）

1. GetSoftwareFence（获取软件围栏）
2. GetZFence（获取Z围栏）
3. ReadSoftwareLimits（读取软件限制）
4. SetSoftwareFence（设置软件围栏）
5. SetZFence（设置Z围栏）

> 6.11 Thermal（热控）

1. EnableHeaterHoldMode（启用加热器保持模式）
2. EnableHeaterStandby（启用加热器待机模式）
3. GetDewPointTemp（获取露点温度）
4. GetHeaterSoak（获取加热器浸泡时间）
5. GetHeaterTemp（获取加热器温度）
6. GetTargetTemp（获取目标温度）
7. GetTemperatureChuckOptions（获取温控夹具选项）
8. GetThermoWindow（获取热窗）
9. HeatChuck（加热夹具）
10. ReadTemperatureChuckStatus（读取夹具温度状态）
11. SendThermoCommand（发送热控命令）
12. SetHeaterSoak（设置加热器浸泡时间）
13. SetHeaterTemp（设置加热器温度）
14. SetTemperatureChuckOptions（设置温控夹具选项）
15. SetThermoWindow（设置热窗）
16. StopHeatChuck（停止加热夹具）

> 6.12 Theta（θ轴）

1. InitTheta（初始化θ轴）
2. MoveTheta（移动θ轴）
3. MoveThetaVelocity（移动θ轴速度）
4. ReadThetaPosition（读取θ轴位置）
5. ReadThetaStatus（读取θ轴状态）
6. SetThetaHome（设置θ轴归位）
7. StopThetaMovement（停止θ轴移动）

> 6.13 ZProfile（Z轴曲线）

1. ClearZProfile（清除Z轴曲线）
2. ReadZProfilePoint（读取Z轴曲线点）
3. SetZProfilePoint（设置Z轴曲线点）

> 6.14 Probes（探针）

1. ClearProbeTablePoint（清除探针台点）
2. GetProbeTableID（获取探针台ID）
3. InitProbe（初始化探针）
4. MoveProbe（移动探针）
5. MoveProbeAlign（移动探针对准）
6. MoveProbeAsync（异步移动探针）
7. MoveProbeContact（移动探针接触）
8. MoveProbeIndex（移动探针索引）
9. MoveProbeLift（移动探针升降）
10. MoveProbeSeparation（移动探针分离）
11. MoveProbeTablePoint（移动探针台点）
12. MoveProbeVelocity（移动探针速度）
13. MoveProbeZ（移动探针Z轴）
14. OrientProbe（定位探针）
15. ReadProbeHeights（读取探针高度）
16. ReadProbeIndex（读取探针索引）
17. ReadProbePosition（读取探针位置）
18. ReadProbeStatus（读取探针状态）
19. ReadProbeTablePoint（读取探针台点）
20. SetProbeBacklash（设置探针背隙）
21. SetProbeHeight（设置探针高度）
22. SetProbeHome（设置探针归位）
23. SetProbeIndex（设置探针索引）
24. SetProbeLED（设置探针LED）
25. SetProbeMode（设置探针模式）
26. SetProbeTablePoint（设置探针台点）
27. StopProbeMovement（停止探针移动）

### 6.内核设置（KernelSetup）  

1. LoadConfigFile（加载配置文件）  
2. ReadKernelData（读取内核数据）  
3. ReplaceFileTreeByKernelData（通过内核数据替换文件树）  
4. ReplaceKernelDataByFileData（通过文件数据替换内核数据）  
5. SaveFileTreeAs（将文件树另存为）  
6. SaveKernelDataAs（将内核数据另存为）  

### 7.加载器

1. AbortJob（中止任务）  
2. ConfirmRecipe（确认配方）  
3. DockCassette（对接载体）  
4. GetAllCassetteRecipes（获取所有载体配方）  
5. GetCassetteRecipe（获取载体配方）  
6. GetCassetteStatus（获取载体状态）  
7. GetIDReaderPos（获取ID读取器位置）  
8. GetJobList（获取任务列表）  
9. GetJobParams（获取任务参数）  
10. GetProbingStatus（获取探测状态）  
11. JobStatus（任务状态）  
12. LoadWafer（加载晶圆）  
13. ProceedJob（继续任务）  
14. ProceedProbing（继续探测）  
15. ProcessStationCloseApplication（处理站关闭应用）  
16. ProcessStationFinish（处理站完成）  
17. ProcessStationGetStatus（获取处理站状态）  
18. ProcessStationGetWaferResult（获取处理站晶圆结果）  
19. ProcessStationInit（初始化处理站）  
20. ProcessStationLoadComplete（处理站加载完成）  
21. ProcessStationLoadRecipe（处理站加载配方）  
22. ProcessStationPauseRecipe（处理站暂停配方）  
23. ProcessStationPrepareForLoad（处理站准备加载）  
24. ProcessStationPrepareForUnLoad（处理站准备卸载）  
25. ProcessStationRecoverError（处理站恢复错误）  
26. ProcessStationStartRecipe（处理站开始配方）  
27. ProcessStationStopRecipe（处理站停止配方）  
28. ProcessStationUnLoadComplete（处理站卸载完成）  
29. ProcessStationVerifyRecipe（处理站验证配方）  
30. ProcessWafer（处理晶圆）  
31. QueryCassetteID（查询载体ID）  
32. QueryWaferID（查询晶圆ID）  
33. QueryWaferInfo（查询晶圆信息）  
34. SetCassetteRecipe（设置载体配方）  
35. SetIDReaderPos（设置ID读取器位置）  
36. StartWaferJob（启动晶圆任务）  
37. TransportWafer（运输晶圆）  
38. UnloadWafer（卸载晶圆）  
39. UpdateCassetteStatus（更新载体状态）  
40. UpdateWaferID（更新晶圆ID）  

### 8.消息服务器 (MsgServer) 

1. ChangeDemoRsp（更改演示响应）  
2. GetDemoMode（获取演示模式）  
3. GetNDriverClientStatus（获取NDriver客户端状态）  
4. InitializationDone（初始化完成）  
5. OverrideCommandTimeout（覆盖命令超时）  
6. SetDemoMode（设置演示模式）  
7. ShutdownVelox（关闭Velox）  

### 9.通知 (Notifications)   

1. AlertNotification（警报通知）  
2. AlignmentModeChange（对准模式变化）  
3. AutoXYModeChange（自动XY模式变化）  
4. BnR_AnalogIONotify（BnR模拟I/O通知）  
5. BnR_AxisNotify（BnR轴通知）  
6. BnR_AxisStatusNotify（BnR轴状态通知）  
7. BnR_ControllerInfoNotify（BnR控制器信息通知）  
8. BnR_InfoNotify（BnR信息通知）  
9. BnR_InputNotify（BnR输入通知）  
10. BnR_OutputNotify（BnR输出通知）  
11. BnR_PositionNotify（BnR位置通知）  
12. BnR_StageNotify（BnR阶段通知）  
13. ButtonPress（按钮按下）  
14. ChuckVacuumChangeRequest（Chuck真空变化请求）  
15. ConfigurationChanged（配置变化）  
16. CryoCmdReady（Cryo命令准备就绪）  
17. KernelCompensationLevelChange（内核补偿级别变化）  
18. KernelCompensationStatusChange（内核补偿状态变化）  
19. KernelConnectionStatus（内核连接状态）  
20. KernelInitStateChange（内核初始化状态变化）  
21. KernelQuietModeChange（内核静默模式变化）  
22. LicenseInfo（许可信息）  
23. LoaderMessage（加载器消息）  
24. MachineStateChange（机器状态变化）  
25. MoveZCombinedStatusChange（Z轴组合状态变化）  
26. NewAccessLevel（新访问级别）  
27. NewProjectFile（新项目文件）  
28. PSLoaderUsage（PS加载器使用）  
29. PSProgressChanged（PS进度变化）  
30. PSStateChanged（PS状态变化）  
31. RegisterProberAppChange（注册探针应用变化）  
32. SaveProjectFile（保存项目文件）  
33. ScopeWorkingStageChanged（光学工作台变化）  
34. ShutdownVeloxNotify（关闭Velox通知）  
35. SoftwareStopChangedNotify（软件停止变化通知）  
36. SubdieChangedNotify（子晶片变化通知）  
37. TTLTestDone（TTL测试完成）  
38. VMProbeCardData（VM探针卡数据）  
39. VMProjectLoaded（VM项目加载）  
40. WaferAlignmentChangedNotify（晶圆对准变化通知）  
41. WaferMapFileLoaded（晶圆图文件加载）  
42. WaferMapFileSaved（晶圆图文件保存）  
43. WMNewCurrentDie（WM新当前晶圆）  
44. WMSetupChange（WM设置变化）  
45. ZoomLevelChange（缩放级别变化）  

### 10.脚本功能 (Scripting) 

1. CloseCommunicator（关闭通讯器）  
2. DoScript（执行脚本）  
3. GetRunStatus（获取运行状态）  
4. StartScript（启动脚本）  

### 11.Spectrum（光谱）   

> 相机（Camera）

1. 获取相机光源（GetCameraLight）  
2. 获取相机视图（GetCameraView）  
3. 设置相机光源（SetCameraLight）  
4. 设置相机静音（SetCameraQuiet）  
5. 设置相机视图（SetCameraView）  

> 自主助手（Autonomous Assistant）

1. 自动化针头搜索（AutomationNeedleSearch）  
2. 自动化参考搜索（AutomationReferenceSearch）  
3. 自动化RF探针搜索（AutomationRFProbeSearch）  
4. 自动化搜索当前芯片（AutomationSearchCurrentDie）  
5. 获取自动化状态（GetAutomationActive）  
6. 获取自动化最后步骤诊断（GetAutomationLastStepDiagnostics）  
7. 获取自动化温度状态（GetAutomationTemperatureStatus）  
8. 获取自动RF校准状态（GetAutoRFCalibrationStatus）  
9. 获取常接触模式状态（GetConstantContactModeStatus）  
10. ISS探针对准（ISSProbeAlign）  
11. 自动XY移动夹具（MoveChuckAutoXY）  
12. 重置自动化（ResetAutomation）  
13. 设置自动化激活（SetAutomationActive）  
14. 设置常接触模式（SetConstantContactMode）  
15. 启动自动化温度控制（StartAutomationTemperature）  
16. 启动自动RF校准（StartAutoRFCalibration）  
17. 停止自动化温度控制（StopAutomationTemperature）  
18. 停止自动RF校准（StopAutoRFCalibration）  
19. VueTrack对准（VueTrackAlign）  

> 常规（General）

1. 检查Spectrum插件（CheckSpectrumPlugin）  
2. 关闭Spectrum（CloseSpectrum）  
3. 启用叠加层（EnableOverlay）  
4. 获取相机归位位置（GetCameraHomePosition）  
5. 获取图案（GetPattern）  
6. 获取光谱数据（GetSpectrumData）  
7. 安全移动定位器（MovePositionersSafe）  
8. 移动到VM位置（MoveToVMPosition）  
9. 读取VM位置（ReadVMPosition）  
10. 设置光谱数据（SetSpectrumData）  
11. 设置光谱远程模式（SetSpectrumRemote）  
12. 显示位置（ShowPosition）  
13. 显示向导（ShowWizard）  
14. 快照图像（SnapImage）  
15. 切换偏移（SwitchOffset）  

> eVue

1. 自动对焦eVue（AutoFocusEVue）  
2. 获取eVue条目数量（EvueGetNumTraceEntries）  
3. 获取eVue触发前条目数量（EvueGetNumTraceEntriesPreTrigger）  
4. 获取eVue条目（EvueGetTraceEntry）  
5. 获取eVue机器状态（EvueGetTraceMachineStatus）  
6. 设置eVue触发前条目数量（EvueSetNumTraceEntriesPreTrigger）  
7. 设置eVue条目捕获停止位（EvueSetTraceCaptureStopBits）  
8. 启动eVue捕获伺服轨迹（EvueStartCaptureServoTrace）  
9. 获取eVue焦点阶段位置（GetEvueFocusStagePos）  
10. 获取eVue焦点阶段范围（GetEvueFocusStageRange）  
11. 获取eVue版本（GetEvueVersion）  
12. 获取eVue变焦级别（GetEvueZoomLevel）  
13. 初始化eVue焦点阶段（InitEvueFocusStage）  
14. 移动eVue焦点阶段（MoveEvueFocusStage）  
15. 运行eVue自动曝光（RunEvueAutoExpose）  
16. 设置eVue变焦级别（SetEvueZoomLevel）  

> 工具（Tools）

1. 辅助对准（AlignAux）  
2. 芯片对准（AlignChip）  
3. 晶圆对准（AlignWafer）  
4. 自动对准（AutoAlign）  
5. 查找特征（FindFeature）  
6. 查找焦点（FindFocus）  
7. 查找焦点平台（FindFocusPlaten）  
8. 查找晶圆中心（FindWaferCenter）  
9. 获取晶圆中心（GetWaferCenter）  
10. 定位归位芯片（LocateHomeDie）  
11. 晶圆预映射（PreMapWafer）  
12. 读取2D矩阵码（Read2DMatrixCode）  
13. 读取条形码（ReadBarCode）  
14. 读取OCR字符串（ReadOcrString）  
15. 读取二维码（ReadQRCode）  
16. 形状追踪器（ShapeTracker）  
17. 训练特征（TrainFeature）  

> 重对准（ReAlign）

1. 偏轴辅助对准（AlignAuxOffAxis）  
2. 偏轴芯片对准（AlignChipOffAxis）  
3. 偏轴自动对准（AutoAlignOffAxis）  
4. 检测晶圆高度（DetectWaferHeight）  
5. 偏轴查找焦点（FindFocusOffAxis）  
6. 获取重对准状态（GetReAlignStatus）  
7. 获取重对准温度状态（GetReAlignTemperatureStatus）  
8. 下一个晶圆（NextWafer）  
9. 探针卡OCS（ProbeCardOCS）  
10. 探针到垫片对准（ProbeToPadAlign）  
11. 重对准（ReAlign）  
12. 启动重对准（StartReAlign）  
13. 启动重对准温度控制（StartReAlignTemperature）  
14. 停止重对准（StopReAlign）  
15. 停止重对准温度控制（StopReAlignTemperature）  
16. 同步相机（SynchronizeCamera）  



### 12.子芯片窗口（SubdieWindow）

1. 捕获自动化探针布局（CaptureAutomationProbeLayout）  
2. 删除自动化探针布局（DeleteAutomationProbeLayout）  
3. 移动到自动化探针布局（MoveToAutomationProbeLayout）  
4. 设置自动化探针布局（SetAutomationProbeLayout）  
5. 显示子芯片窗口（ShowSubdieWindow）  

### 13.表格视图（TableView）  

1. 关闭表格视图（CloseTableView）  
2. 读取夹具站点位置（ReadChuckSitePosition）  
3. 读取夹具子站点位置（ReadChuckSubsitePosition）  
4. 读取探针站点位置（ReadProbeSitePosition）  
5. 读取显微镜站点位置（ReadScopeSitePosition）  
6. 步进夹具站点（StepChuckSite）  
7. 步进夹具子站点（StepChuckSubsite）  
8. 步进探针站点（StepProbeSite）  
9. 步进显微镜站点（StepScopeSite）    

### 14.测试仪（Tester）   

1. 批次结束（EndOfLot）  
2. 晶圆结束（EndOfWafer）  
3. 新测试仪项目（NewTesterProject）  
4. 启动测量（StartMeasurement）  
5. 测试仪中止（TesterAbort）  
6. 测试仪中止晶圆（TesterAbortWafer）  
7. 测试仪盒信息（TesterCassetteInfo）  
8. 验证批次ID（VerifyLotID）  
9. 验证探针卡（VerifyProbecard）  
10. 验证产品ID（VerifyProductID）  
11. 验证项目（VerifyProject）  
12. 验证SOT准备就绪（VerifySOTReady）  
13. 验证基板ID（VerifySubstrateID）  
14. 验证用户ID（VerifyUserID）  
15. 验证晶圆启动（VerifyWaferStart）    


### 15.工具栏（Toolbar）  

1. 显示工具栏配置（ToolbarShowConfig）    

### 16.TTL（TTL）

1. 取消TTL测试（CancelTTLTest）  
2. 执行TTL测试（DoTTLTest）  
3. 获取TTL线路（GetTTLLines）  
4. 获取TTL状态（GetTTLStatus）  
5. 设置TTL线路（SetTTLLine）  
6. 启动TTL测试（StartTTLTest）  

### 17.晶圆地图 (WaferMap)


1. WaferMap (WaferMap)
2. Dies (芯片)
3. AssignMapBins (分配映射箱)
4. BinMapDie (芯片映射箱)
5. BinStepDie (芯片步进映射)
6. DoInkerRun (执行Ink运行)
7. GetDieDataAsColRow (获取芯片数据为列行格式)
8. GetDieDataAsNum (获取芯片数据为数字格式)
9. GetDieInfo (它用于获取与指定 Die 相关的测试站点数量。) 
10. GetDieLabel (获取芯片标签)
11. GetDieLabelAsNum (获取芯片标签为数字格式)
12. GetDieResult (获取芯片结果)
13. GetDieResultAsNum (获取芯片结果为数字格式)
14. GetDieStatus (获取芯片状态)
15. GetNumSelectedDies (获取选定芯片数量)
16. GetPreMappedDieInfo (获取预先映射的芯片信息)
17. GetSelectedDieCoords (获取选定芯片的坐标)
18. GetSelectiveDieAlignmentDieStatus (获取选择性芯片对准状态)
19. GetSelectiveDieSoakingDieStatus (获取选择性芯片浸泡状态)
20. SelectAllDiesForProbing (选择所有芯片进行探测)
21. SetDieDataAsColRow (设置芯片数据为列行格式)
22. SetDieDataAsNum (设置芯片数据为数字格式)
23. SetDieLabel (设置芯片标签)
24. SetDieLabelAsNum (设置芯片标签为数字格式)
25. SetDieResult (设置芯片结果)
26. SetDieResultAsNum (设置芯片结果为数字格式)
27. SetDieStatus (设置芯片状态)
28. SetSelectiveDieAlignmentDieStatus (设置选择性芯片对准状态)
29. SetSelectiveDieSoakingDieStatus (设置选择性芯片浸泡状态)
30. StepFailedBack (步骤失败后退)
31. StepFailedForward (步骤失败前进)
32. StepFirstDie (步骤第一颗芯片)
33. StepNextDie (步骤下一颗芯片)
34. StepNextDieOffset (步骤下一颗芯片偏移)
35. StepToDie (步骤到达芯片)
36. Subdies (子芯片)
37. AddSubDie (添加子芯片)
38. BinSubDie (子芯片映射箱)
39. DeleteAllSubDie (删除所有子芯片)
40. DeleteSubDie (删除子芯片)
41. DeleteSubDie2 (删除子芯片2)
42. GetDieMapResult (获取芯片映射结果)
43. GetDieMapResultAsNum (获取芯片映射结果为数字格式)
44. GetDieRefPoint (获取芯片参考点)
45. GetSubDieData (获取子芯片数据)
46. GetSubDieDataAsColRow (获取子芯片数据为列行格式)
47. GetSubDieDataAsNum (获取子芯片数据为数字格式)
48. GetSubDieLabel (获取子芯片标签)
49. GetSubDieLabelAsNum (获取子芯片标签为数字格式)
50. GetSubDieStatus (获取子芯片状态)
51. SetDieMapResult (设置芯片映射结果)
52. SetDieMapResultAsNum (设置芯片映射结果为数字格式)
53. SetDieRefPoint (设置芯片参考点)
54. SetSubDieData (设置子芯片数据)
55. SetSubDieDataAsColRow (设置子芯片数据为列行格式)
56. SetSubDieDataAsNum (设置子芯片数据为数字格式)
57. SetSubDieLabel (设置子芯片标签)
58. SetSubDieLabelAsNum (设置子芯片标签为数字格式)
59. SetSubDieStatus (设置子芯片状态)
60. StepNextSubDie (步骤下一子芯片)
61. Maps (地图)
62. CloseWaferMap (关闭晶圆映射)
63. ConvertToAlphas (转换为字母)
64. GetActiveLayer (获取当前层)
65. GetCurrentBin (获取当前箱)
66. GetHomeDieOffset (获取参考芯片偏移)
67. GetLotID (获取批次ID)
68. GetMapDims (获取地图尺寸)
69. GetMapHome (获取地图首页)
70. GetMapName (获取地图名称)
71. GetMapOrientation (获取地图方向)
72. GetMapRoute (获取地图路径)
73. GetProductID (获取产品ID)
74. GetRectMapParams (获取矩形地图参数)
75. GetSelectiveDieAlignmentMode (获取选择性芯片对准模式)
76. GetSelectiveDieSoakingMode (获取选择性芯片浸泡模式)
77. GetUsePositionerSubdie (获取使用定位器子芯片)
78. GetWaferID (获取晶圆ID)
79. GetWaferInfo (获取晶圆信息)
80. GetWaferMapMode (获取晶圆映射模式)
81. GetWaferMapParams (获取晶圆映射参数)
82. GetWaferMapParams2 (获取晶圆映射参数2)
83. GetWaferNum (获取晶圆编号)
84. GetWaferTestAngle (获取晶圆测试角度)
85. GoToWaferHome (前往晶圆首页)
86. LoadPreMappedDiesTable (加载预映射的芯片表)
87. MapEdgeDies (映射边缘芯片)
88. NewWaferMap (新建晶圆映射)
89. OpenWaferMap (打开晶圆映射)
90. ReadMapPosition (读取地图位置)
91. ReadMapPosition2 (读取地图位置2)
92. ReadMapYield (读取地图产量)
93. SaveMapFile (保存地图文件)
94. SetActiveLayer (设置当前层)
95. SetCurrentBin (设置当前箱)
96. SetG85FileOptions (设置G85文件选项)
97. SetLotID (设置批次ID)
98. SetMapHome (设置地图首页)
99. SetMapOrientation (设置地图方向)
100. SetMapRoute (设置地图路径)
101. SetProductID (设置产品ID)
102. SetRectMapParams (设置矩形地图参数)
103. SetRefDieOffset (设置参考芯片偏移)
104. SetSelectiveDieAlignmentMode (设置选择性芯片对准模式)
105. SetSelectiveDieSoakingMode (设置选择性芯片浸泡模式)
106. SetWaferID (设置晶圆ID)
107. SetWaferMapMode (设置晶圆映射模式)
108. SetWaferMapParams (设置晶圆映射参数)
109. SetWaferMapParams2 (设置晶圆映射参数2)
110. SetWaferNum (设置晶圆编号)
111. SetWaferTestAngle (设置晶圆测试角度)
112. SetWindowState (设置窗口状态)
113. SyncMapHome (同步地图首页)
114. Z-Profiling (Z轴轮廓)
115. DoWaferProfiling (执行晶圆轮廓)
116. DoWaferProfilingOffAxis (执行离轴晶圆轮廓)
117. GetWaferProfileOptions (获取晶圆轮廓选项)
118. GetWaferProfilingStatus (获取晶圆轮廓状态)
119. GetZProfilingStatus (获取Z轴轮廓状态)
120. OpenZProfileFile (打开Z轴轮廓文件)
121. ReadZProfile (读取Z轴轮廓)
122. SaveZProfileFile (保存Z轴轮廓文件)
123. SetWaferProfileOptions (设置晶圆轮廓选项)
124. SetZProfile (设置Z轴轮廓)
125. StartWaferProfiling (开始晶圆轮廓)
126. StartZProfiling (开始Z轴轮廓)
127. StopWaferProfiling (停止晶圆轮廓)
128. StopZProfiling (停止Z轴轮廓)
129. ZProfileWafer (Z轴轮廓晶圆)
130. Clusters (簇)
131. GetClusterDieStatus (获取簇芯片状态)
132. GetClusterInfo (获取簇信息)
133. GetClusterParams (获取簇参数)
134. GetNumSelectedClusters (获取选定簇数量)
135. GetSelectedClusterCoords (获取选定簇坐标)
136. ReadClusterPosition (读取簇位置)
137. SetClusterDieStatus (设置簇芯片状态)
138. SetClusterParams (设置簇参数)
139. StepFailedClusterBack (簇步骤失败后退)
140. StepFailedClusterForward (簇步骤失败前进)
141. StepFirstCluster (步骤第一簇)
142. StepNextCluster (步骤下一簇)
143. Bins (箱)
144. ClearAllBins (清除所有箱)
145. GetBinCode (获取箱代码)
146. GetBinTableSize (获取箱表大小)
147. OpenBinCodeTable (打开箱代码表)
148. SaveBinCodeTable (保存箱代码表)
149. SetBinCode (设置箱代码)
150. SetBinTableSize (设置箱表大小)

### 18.Z轴分析(ZProfiling)

1. ZProfiling (Z轴轮廓)
2. AddZProfilePoint (添加Z轴轮廓点)
3. CloseZProfiling (关闭Z轴轮廓)
4. DeleteZProfilePoint (删除Z轴轮廓点)
5. GetZProfileOptions (获取Z轴轮廓选项)
6. GetZProfileOrigin (获取Z轴轮廓原点)
7. GetZProfilePointStatus (获取Z轴轮廓点状态)
8. GetZProfileStartPoint (获取Z轴轮廓起始点)
9. PreciseZProfile (精确Z轴轮廓)
10. SetZProfileOptions (设置Z轴轮廓选项)
11. SetZProfileOrigin (设置Z轴轮廓原点)
12. SetZProfilePointStatus (设置Z轴轮廓点状态)
13. SetZProfileStartPoint (设置Z轴轮廓起始点)
14. StepFirstZProfilePoint (步骤第一Z轴轮廓点)
15. StepNextZProfilePoint (步骤下一Z轴轮廓点)

### 19.Enumerations (枚举)  

2. AccessLevel (访问级别)
3. AlignmentMode (对准模式)
4. AnalogIO (模拟I/O)
5. AuxSiteType (辅助站点类型)
6. Axis (轴)
7. BnR_AxisState (BnR轴状态)
8. BnR_AxisStatus (BnR轴状态)
9. BnR_InternalInfo (BnR内部信息)
10. BnR_MessageType (BnR消息类型)
11. BnR_ScopeTypeCM300 (BnR CM300示波器类型)
12. BnR_ScopeTypeS200 (BnR S200示波器类型)
13. CameraConnectType (相机连接类型)
14. CBoxSpeed (CBox速度)
15. Comp (补偿)
16. CompensationType (补偿类型)
17. CompLayer (补偿层)
18. ConnectionCheckResult (连接检查结果)
19. ConnectionCheckType (连接检查类型)
20. ConnectionState (连接状态)
21. ControllerData (控制器数据)
22. ControllerInfo (控制器信息)
23. ControllerType (控制器类型)
24. CoordSystem (坐标系统)
25. DefectType (缺陷类型)
26. ElitePurgeCommand (Elite清除命令)
27. ErrorSeparator (错误分隔符)
28. EServerControllerConfig (EServer控制器配置)
29. ExternalModeType (外部模式类型)
30. FenceForm (围栏形式)
31. FenceType (围栏类型)
32. FocusStage (聚焦阶段)
33. GPIBBoardType (GPIB板类型)
34. IDReaderPos (ID读取器位置)
35. InstallType (安装类型)
36. LoadPosition (加载位置)
37. LoadPosType (加载位置类型)
38. Location (位置)
39. MachineState (机器状态)
40. ManualPlatenState (手动托盘状态)
41. MaterialStateType (材料状态类型)
42. MoveZCombinedStatus (Z轴移动组合状态)
43. MultiLineVacuum (多线真空)
44. NanoChamberState (纳米室状态)
45. NucleusProberType (核心探针类型)
46. OperationalMode (操作模式)
47. PerformanceMode (性能模式)
48. Polarity (极性)
49. PosRef (位置参考)
50. PresetHeight (预设高度)
51. ProbeFrequency (探针频率)
52. ProberType (探针类型)
53. ProberTypeSphere (球形探针类型)
54. ProbeType (探针类型)
55. ProcessStationJobStatus (工艺站作业状态)
56. ProcessStationModuleType (工艺站模块类型)
57. ProcessStationProbingType (工艺站探测类型)
58. ProcessStationProceedJob (工艺站继续作业)
59. ProcessStationSlotStatus (工艺站槽状态)
60. ProcessStationStatusType (工艺站状态类型)
61. ProcessStationWaferIDStatus (工艺站晶圆ID状态)
62. ResetMode (重置模式)
63. RunningMode (运行模式)
64. SCPIStage (SCPI阶段)
65. SensorType (传感器类型)
66. SetupDialogType (设置对话框类型)
67. Side (侧面)
68. SoftwareFenceType (软件围栏类型)
69. SoftwarePath (软件路径)
70. Stage (阶段)
71. SystemIO (系统I/O)
72. TaskStatus (任务状态)
73. TemperatureChuckStatus (温度夹具状态)
74. TempUnit (温度单位)
75. Term (术语)
76. TraceMode (跟踪模式)
77. Unit (单位)
78. WLEMOption (WLEM选项)
79. WLEMSensor (WLEM传感器)
80. WLEMState (WLEM状态)



