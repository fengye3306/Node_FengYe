
## 框架API级优化

### 调整Item的索引模式   

图元位置默认通过BSP Tree来进行存储，特点是查找效率高，也就是通过items()或者itemAt()来定位查找图元的时候会比较快。   
但在这种情况下，如果一次性移动很多图元，那么框架就可能需要更新整个二叉树，这个更新操作是很耗时的。   

```cpp
// 禁用索引优化
scene()->setItemIndexMethod(QGraphicsScene::NoIndex);
```


### 绘制缓存  

如果某个图元的绘制函数是很耗时的，我们可以开启该图元的缓存模式来进行优化，默认的情况，缓存模式是被关闭的。缓存模式开启的函数如下：  

```cpp
void QGraphicsItem::setCacheMode(CacheMode mode, const QSize &logicalCacheSize = QSize());
```

* NoCache	默认缓存模式被禁用，每次图元需要重绘的时候 QGraphicsItem::paint()函数都会被调用。
* ItemCoordinateCache	给图元指定一个QSize()大小的缓存，缓存里面的图像可以用来进行后续的绘制和位移变换。如果想调整缓存图像的质量和分辨率，可以通过调用setCacheMode()分配一块更大的空间。
* DeviceCoordinateCache	绘制设备级别的缓存优化，主要适用于那些可以移动，但不会发生位移变化(旋转、缩放、剪切)的图元。如果图元发生了位移变化，那么缓存会自动更新。和ItemCoordinateCache不同 DeviceCoordinateCache不需要指定缓存空间大小，它始终以最高质量进行渲染。



