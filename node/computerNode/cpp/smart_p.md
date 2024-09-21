## std::shared_ptr

*shared_ptr 共享对象的所有权，并管理对象的生命周期*  

* **共享所有权：**多个 shared_ptr 可以指向同一个对象，它们共享该对象的所有权。
* **自动管理内存：**当最后一个 shared_ptr 被销毁或重置时，所指向的对象会自动删除。
* **引用计数：**每个 shared_ptr 会维护一个引用计数，当有新的 shared_ptr 指向相同的对象时，引用计数会增加。当 shared_ptr 被销毁时，引用计数减少到 0，才会释放内存。

```cpp
// 创建 shared_ptr
std::shared_ptr<int> sp1 = std::make_shared<int>(10);  
// sp2 共享 sp1 的所有权
std::shared_ptr<int> sp2 = sp1;                        
// 引用计数为 2，只有 sp1 和 sp2 都销毁时，内存才会被释放
```

## std::weak_ptr

*weak_ptr 只是一个弱引用，不管理对象的生命周期*   

* **不共享所有权：**weak_ptr 不会增加对象的引用计数，它只是一个对 shared_ptr 管理的对象的弱引用。因为它不拥有对象，所以不会影响对象的生命周期。  
* **避免循环引用：**weak_ptr 主要用于打破循环引用问题。如果两个或多个 shared_ptr 互相引用对方，它们的引用计数永远不会减为 0，导致内存泄漏。weak_ptr 用于引用但不参与内存管理，避免这种情况。
* **需要提升：**由于 weak_ptr 不保证对象的存在，使用它时需要先调用 lock() 来生成一个 shared_ptr。如果对象已经被销毁，lock() 返回一个空的 shared_ptr。


```cpp
// 创建 shared_ptr
std::shared_ptr<int> sp1 = std::make_shared<int>(10);  
// 创建 weak_ptr，指向 sp1
std::weak_ptr<int> wp1 = sp1;  

if (std::shared_ptr<int> sp2 = wp1.lock()) {
    // 如果 sp1 仍然有效，则可以通过 sp2 使用对象
    std::cout << *sp2 << std::endl;
} else {
    // sp1 已被销毁
    std::cout << "Object no longer exists" << std::endl;
}
```