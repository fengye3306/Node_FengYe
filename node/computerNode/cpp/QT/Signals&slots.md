
# 槽函数重载

```cpp
// /CloudCompare/ccViewer/ccviewer.cpp
connect(
    m_glWindow->signalEmitter(),
    &ccGLWindowSignalEmitter::filesDropped,				 
    this,	
    qOverload<QStringList>(&ccViewer::addToDB), 
    Qt::QueuedConnection);
```

`qOverload<QStringList>(&ccViewer::addToDB)`   
实现对指定`ccViewer::addToDB(QStringList)`接口进行绑定。


# 信号槽实现



