

## 框架  

### 框架引入

```py
from Qt import QtWidgets
from NodeGraphQt import NodeGraph, BaseNode
```

## 节点   

### 节点重写  

```py
# create a node class object inherited from BaseNode.
class MNode(BaseNode):

    # unique node identifier domain.
    # 唯一标识符
    __identifier__ = 'io.github.jchanvfx'

    # initial default node name.
    # 节点首次创建或初始化时被自动分配的名称
    NODE_NAME = 'Foo Node'

    # 构造函数
    def __init__(self):
        super(MNode, self).__init__()
        # create an input port.
        self.add_input('in', color=(180, 80, 0))
        # create an output port.
        self.add_output('out')
```

* **self等价于c++类中的this**,其指向了当前对象实例。

> 重写节点添加窗口  

```cpp
items = ['apples', 'bananas', 'pears', 'mangos', 'oranges']
self.add_combo_menu('my_list', 'My List', items)
```




### 节点创建  

```py
# create two nodes.
node_a = graph.create_node('io.github.jchanvfx.MNode', name='node A')
node_b = graph.create_node('io.github.jchanvfx.MNode', name='node B', pos=(300, 50))
```

### 节点相连

```py
# node_a节点 输出端口0 连上 节点node_b输入端口0
node_a.set_output(0, node_b.input(0))
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