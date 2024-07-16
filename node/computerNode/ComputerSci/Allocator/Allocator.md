# 对象池   


* 池化技术特点共同特点：提前创建资源，以备不时之需。   
分配内存，创建进程、线程都设计到用户态到内核态的转换，频繁申请，销毁是对性能的空耗。   

* 对象池实现本质在于：  
申请一个大额内存空间，再把大空间切为小空间。   
当需要使用时候直接分配使用，不再需要额外向系统申请内存空间，使用过后放回池子中。   

![logo](./img/01.png ':size=WIDTHxHEIGHT')

## 源码地址

```link
https://github.com/fengye3306/ObjectPool
```

## 多策略对象池框架设计  

通过实例化不同的策略以完成多策略对象池框架设计。  
而外部类通过重写new\delete实现，将由原来的申请释放资源，转而通过对象池申请释放空间。   

> 对象池策略接口

```cpp
#pragma once 

namespace Object{

template <typename T>
class Allocator {
public:
    virtual void* allocator() = 0;      // 分配内存       
    virtual void  deallocate(T *p) = 0; // 解绑空间
};

}

```

> 对象池   

```cpp

# pragma once 

#include <ObjectPool/allocator.hpp>
#include <stdexcept>

namespace Object{

template <typename T,typename Allocator>
class ObjectPool {
public:
    ObjectPool()  = default;
    ~ObjectPool() = default;

    // 分配空间
    void* allocator(size_t n){
        if(sizeof(T) != n){
            throw std::bad_alloc();
        }
        return m_allocator.allocator();
    };

    // 解绑空间
    void deallocate(void *p){
        m_allocator.deallocate(static_cast<T *>(p));
    }
private:
    // 策略
    Allocator m_allocator;                      
};

}
```

> 外部类池化Object::ArrayAllocator

```cpp
# pragma once 

#include <ObjectPool/allocator.hpp>
#include <ObjectPool/ObjectPool.hpp>
#include <ObjectPool/allocator/array_allocator.hpp>

const int ObjectPool_Max_Size = 10;

class Class_Test_Array{

    // Object::ArrayAllocator 绑定 数组策略
    typedef 
        Object::ObjectPool<Class_Test_Array, Object::ArrayAllocator<Class_Test_Array,ObjectPool_Max_Size>> 
        ObjectPool;
    static ObjectPool pool;  
public:
    Class_Test_Array()  = default;
    ~Class_Test_Array() = default;

    void *operator new(size_t n) { return pool.allocator(n);};
    void operator delete(void *p){ pool.deallocate(p);};
};

Class_Test_Array::ObjectPool Class_Test_Array::pool;
```



## 数组策略、堆策略、栈策略  

从数组策略，到堆策略再到栈策略，本质都是在系统启动时预申请一大块固定大小为`Sizeof(T)*N，其中T为对象大小，N为对象池容量上限`内存，之后被池化的类通过重载new\delete以使得从此固定大空间中割取小块内存以使用，析构时将内存块归还于对象池。         

其中，数组策略、堆策略、栈策略本质的区别就在于，管理 分割和归还 内存块的算法不同！  
算法不同而引发性能区别。其中性能上栈优于堆,堆优于队列，栈策略拥有o1复杂度，接近完美。  
算法实现请阅读github项目源码`include/ObjectPool/allocator/`allocator路径用于存放策略。  

## 区块策略  

数组策略、堆策略、栈策略 有一个共同问题 ———— 策略使得对象池的大小固定。  
实际场景中服务的峰值并不一定直白可知，过大的池浪费内存，过小池无区块可用引发问题。      

区块策略在于**不限制对象池大小上限**。   

![logo](./img/02.png ':size=WIDTHxHEIGHT')






