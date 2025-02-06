

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

