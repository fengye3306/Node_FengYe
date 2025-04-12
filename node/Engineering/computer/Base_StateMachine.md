
契机是工作中需要根据业务来推进设备的启停，根据人员操作来配置设备组状态。

## QStateMachine  

Qt对于状态机问题给出了自己的答案————QStateMachine框架。   
框架中各个组件各司其职，组成了一个完善的状态机系统。   

```msg

| 组件                  | 职责                                      |
|---------------------|--------------------------------------|
| **QStateMachine**     | 管理状态、调度转换、启动／停止整个状态机     |
| **QAbstractState**    | 状态的基类，支持层次化、并行、进入／离开钩子  |
| **QState**            | 可执行状态节点，定义属性变化与转换             |
| **QFinalState**       | 结束状态，进入后发出完成信号                  |
| **QHistoryState**     | 历史状态，记忆并恢复子状态                  |
| **QAbstractTransition** | 转换基类，定义条件与动作                   |
| **QSignalTransition**   | 基于信号触发的转换                       |
| **QEventTransition**    | 基于事件触发的转换                       |
| **QTimerTransition**    | 基于定时器超时触发的转换（Qt 5.8+）      |
```


