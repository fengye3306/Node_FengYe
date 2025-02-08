
## 框架  

### 框架引入

```py
from Qt import QtWidgets
from NodeGraphQt import NodeGraph, BaseNode
```

## 自制节点工厂  

> 实现一个节点工厂必要配置 

```py
# create a node class object inherited from BaseNode.
class MNode(BaseNode):

    # 唯一标识符
    # unique node identifier domain.
    __identifier__ = 'io.github.jchanvfx'

    # initial default node name.
    # 节点首次创建或初始化时被自动分配的名称
    NODE_NAME = 'Foo Node'

    # 构造函数
    # construct
    def __init__(self):
        super(MNode, self).__init__()
```

* **self等价于c++类中的this**,其指向了当前对象实例。

### 小部件嵌入  

文档：`http://chantonic.com/NodeGraphQt/api/node_widgets.html#embedded-node-widgets` 

> 复选框 combbox 

```py
# 工厂生产  
items = ['apples', 'bananas', 'pears', 'mangos', 'oranges']
self.add_combo_menu('my_list', 'My List', items)
```

```py
# 节点对象可通过set_property 配置所有复选框 例如
node = MyListNode()
node.set_property('my_list', 'mangos')
```

### 自定义窗口嵌入

将qt窗口封装于 NodeBaseWidget 派生类中以 加入节点框架, 将NodeBaseWidget 派生类组合至某一特定节点工厂。  


```py
class NodeWidget_Show2DImage(NodeBaseWidget):

    def __init__(self, parent=None):
        super(NodeWidget_Show2DImage, self).__init__(parent)
        # Wid_ShowImage 是你实现的一个2d图像窗口
        self.set_custom_widget(Wid_ShowImage())

    def get_value(self):
        widget = self.get_custom_widget()
        return 'null'
    
    def set_value(self, value):
        return

class Node_ShowImage(BaseNode):

    __identifier__  = 'Node.MV'
    NODE_NAME       = 'show image'

    def __init__(self):
        super(Node_ShowImage, self).__init__()
        
        node_widget = NodeWidget_Show2DImage(self.view)
        self.add_custom_widget(node_widget, tab='Custom')
```


### 节点端口  

```py
def __init__(self):
    super(MyNode, self).__init__()

    # input
    self.add_input('foo')   # 端口名foo
    self.add_input('hello') # 端口名hello

    # output
    self.add_output('bar')
    self.add_output('world')
```

> 自定义端口样式  

```py
####################################
实现自定义端口样式,此例为一个三角形
####################################

# 绘制函数
def draw_triangle_port(painter, rect, info):
    """
    Custom paint function for drawing a Triangle shaped port.

    Args:
        painter (QtGui.QPainter): painter object.
        rect (QtCore.QRectF): port rect used to describe parameters needed to draw.
        info (dict): information describing the ports current state.
            {
                'port_type': 'in',
                'color': (0, 0, 0),
                'border_color': (255, 255, 255),
                'multi_connection': False,
                'connected': False,
                'hovered': False,
            }
    """
    painter.save()

    # create triangle polygon.
    size = int(rect.height() / 2)
    triangle = QtGui.QPolygonF()
    triangle.append(QtCore.QPointF(-size, size))
    triangle.append(QtCore.QPointF(0.0, -size))
    triangle.append(QtCore.QPointF(size, size))

    # map polygon to port position.
    transform = QtGui.QTransform()
    transform.translate(rect.center().x(), rect.center().y())
    port_poly = transform.map(triangle)

    # mouse over port color.
    if info['hovered']:
        color = QtGui.QColor(14, 45, 59)
        border_color = QtGui.QColor(136, 255, 35)
    # port connected color.
    elif info['connected']:
        color = QtGui.QColor(195, 60, 60)
        border_color = QtGui.QColor(200, 130, 70)
    # default port color
    else:
        color = QtGui.QColor(*info['color'])
        border_color = QtGui.QColor(*info['border_color'])

    pen = QtGui.QPen(border_color, 1.8)
    pen.setJoinStyle(QtCore.Qt.MiterJoin)

    painter.setPen(pen)
    painter.setBrush(color)
    painter.drawPolygon(port_poly)

    painter.restore()


# 实际端口调用时，可以通过加载对应函数决定端口样式   
class Node_ShowImage(BaseNode):

    __identifier__  = 'Node.MV'
    NODE_NAME       = 'show image'

    def __init__(self):
        super(Node_ShowImage, self).__init__()
        node_widget = NodeWidget_Show2DImage(self.view)
        self.add_custom_widget(node_widget, tab='Custom')

        # 端口样式由painter_func 形参决定
        self.add_input('triangle', painter_func=draw_triangle_port)
        self.add_output('triangle', painter_func=draw_triangle_port)
```


### 链接约束  

```py
from NodeGraphQt.constants import PortTypeEnum

class Node_ShowImage(BaseNode):

    __identifier__  = 'Node.MV'
    NODE_NAME       = 'show image'

    def __init__(self):
        super(Node_ShowImage, self).__init__()
        node_widget = NodeWidget_Show2DImage(self.view)
        self.add_custom_widget(node_widget, tab='Custom')

        in_Run_驱动 = self.add_input('Run')
        in_Run_驱动.add_accept_port_type(
                    port_name='Run',
                    port_type=PortTypeEnum.OUT.value,
                    node_type='Node.MV.Node_ShowImage'
                )
        out_Run_驱动 = self.add_output('Run')
        out_Run_驱动.add_accept_port_type(
                    port_name='Run',
                    port_type=PortTypeEnum.IN.value,
                    node_type='Node.MV.Node_ShowImage'
                )

        in_Mat_图片获取  = self.add_input('Mat')
        in_Mat_图片获取.add_accept_port_type(
                    port_name='Mat',
                    port_type=PortTypeEnum.OUT.value,
                    node_type='Node.MV.Node_ShowImage'
                )
        out_Mat_图片输出 = self.add_output('Mat')
        out_Mat_图片输出.add_accept_port_type(
                    port_name='Mat',
                    port_type=PortTypeEnum.IN.value,
                    node_type='Node.MV.Node_ShowImage'
                )
```



## 视图  

### 创建视图类

```py
app = QtWidgets.QApplication([])

# create node graph controller.
graph = NodeGraph()

# register the FooNode node class.
graph.register_node(FooNode)

# show the node graph widget.
graph_widget = graph.widget
graph_widget.show()

app.exec_()
```

### 视图挂载节点工厂

```py
# register the FooNode node class.
graph.register_node(FooNode)
```

### 视图创建节点  

```py
# 创建节点 node_a 
#         类型： io.github.jchanvfx.FooNode
#         标题： n_a
node_a = graph.create_node('io.github.jchanvfx.FooNode', name='n_a')
```

### 视图配置链接样式  

```py
from NodeGraphQt import NodeGraph
from NodeGraphQt.constants import PipeLayoutEnum

graph = NodeGraph()
# 更改枚举值
graph.set_pipe_style(PipeLayoutEnum.ANGLE.value)
```


## 属性面板   

PropertiesBinWidget 节点属性管理器。 是抽取节点属性管理问题的设计模式产物。  
对场景基类进行重写,可以实现使得当任意节点被双击后调出节点属性管理器。  

```py
class MyNodeGraph(NodeGraph):

    def __init__(self, parent=None):
        super(MyNodeGraph, self).__init__(parent)

        # _prop_bin 属性
        self._prop_bin = PropertiesBinWidget(node_graph=self)
        self._prop_bin.setWindowFlags(QtCore.Qt.Tool)

        # wire signal.
        self.node_double_clicked.connect(self.display_prop_bin)

    def display_prop_bin(self, node):
        """
        function for displaying the properties bin when a node
        is double clicked
        """
        if not self._prop_bin.isVisible():
            self._prop_bin.show()
```

