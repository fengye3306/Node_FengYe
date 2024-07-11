
## simple_graph_model

*教你如何自定义节点管理器，触碰到了架构的底层*

1. 继承自 节点管理器`QtNodes::AbstractGraphModel`,定义自己的节点管理器
2. 展示了底层的节点的链接、创建、撤销重回 等操作实现的本质
 


## resizable_images

*教你怎么自定义节点，此处实现了图片加载与传递两个节点并将之加入到官方定义的节点管理器 DataFlowGraphModel : public AbstractGraphModel中。属于架构的应用层实现*

## lock_nodes_and_connections  

*教你设置视图锁，两把锁。 一把锁定整个视图不可被操作，另一把仅仅锁住视图模块的链接不可被修改*  

## dynamic_ports

*PortAddRemoveWidget : public QWidget 创生于节点模块中，这里演示通过窗口的触发调用节点模块接口新增或是删除节点。*   


## styles  

*如何使用接口改变窗口样式*   
GraphicsViewStyle::setStyle
NodeStyle::setNodeStyle
ConnectionStyle::setConnectionStyle

## vertical_layout  
端口的横向排布或是纵向排布    

BasicGraphicsScene::setOrientation  更改这个排布接口，会直接更改视图内所有的节点

## connection_colors  

链接线的颜色  

```c++
// 需要开启全局的 线颜色各异
static void setStyle()
{
    ConnectionStyle::setConnectionStyle(
        R"(
  {
    "ConnectionStyle": {
      "UseDataDefinedColors": true
    }
  }
  )");
}

```