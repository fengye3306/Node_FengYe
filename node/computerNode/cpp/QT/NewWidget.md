# QWidget 

* `adjustSize` 窗口自适应完全显示  

> QWidget添加菜单栏

```c++
    
    QWidget mainWidget; // 主窗口
    QVBoxLayout *layout_QVBox = new QVBoxLayout(&mainWidget);   

    // 添加菜单
    auto menuBar = new QMenuBar();
    QMenu *menu = menuBar->addMenu("File");
    auto saveAction = menu->addAction("Save Scene");

    layout_QVBox->addWidget(menuBar);

    // 视窗
    auto wid_view = new QWidget();
    layout_QVBox->addWidget(vwid_viewiew);

    layout_QVBox->setContentsMargins(0, 0, 0, 0);
    layout_QVBox->setSpacing(0);
```