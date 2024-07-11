

## 数据流是如何运作的  

* NodeDelegateModelRegistry 
    托管所有的节点模块，是节点模块管理器

    

* DataFlowGraphModel    
托管所有的节点模块  



## 自定义模块的端口数据   

对于 一个节点模块，如何自定义它的输入输出节点   

```c++
    /*通过重载节点对象两个接口完成自定义输入输出节点
    class 我的节点模块 : public NodeData 
*/

    // 设定模块输入输出端口个数
    unsigned int nPorts(PortType const portType) const override
    {
        unsigned int result = 1;

        switch (portType) {
        case PortType::In:
            // 输入端口有几个值？
            result = 2;
            break;

        case PortType::Out:
            // 输出端口有几个值？    
            result = 5;
            break;
        case PortType::None:
            break;
        }

        return result;
    }

    // 设定模块输入输出端口 端口数据类型
    NodeDataType dataType(PortType const portType, PortIndex const portIndex) const override
    {
        switch (portType) {
        case PortType::In:
            switch (portIndex) {
            case 0:
                // 数据类型
                return MyNodeData().type();
            case 1:
                // 数据类型
                return SimpleNodeData().type();
            }
            break;

        case PortType::Out:
            switch (portIndex) {
            case 0:
                // 数据类型
                return MyNodeData().type();
            case 1:
                // 数据类型
                return SimpleNodeData().type();
            // 更多的定义.......
            }
            break;

        case PortType::None:
            break;
        }
        // FIXME: control may reach end of non-void function [-Wreturn-type]
        return NodeDataType();
    }
```


## 节点多链&单链  


![节点](./img/01_.png ':size=WIDTHxHEIGHT')  

如图，部分节点支持多链接，而部分节点仅仅支持单链接  
