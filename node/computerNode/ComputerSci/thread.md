# 多线程  

C语言由POSIX标准下`pthread`以提供多线程支持。   

```Msg
POSIX（可移植操作系统接口，Portable Operating System Interface）标准是一组针对操作系统的接口标准，由IEEE（电气和电子工程师协会）制定。POSIX标准旨在保证不同操作系统之间的可移植性，使得应用程序能够在不同的UNIX系统和类UNIX系统（如Linux、macOS）上运行而无需进行大量修改。POSIX标准涵盖了文件系统操作、进程管理、线程管理、信号处理、网络编程等多个方面。

pthread（POSIX threads，即POSIX线程）是POSIX标准中的一个重要部分，用于在程序中实现多线程编程。pthread提供了一组API（应用程序编程接口），使得开发者可以创建、管理和同步线程。通过使用pthread，开发者可以更高效地利用多核处理器的性能，编写并发和并行的程序。
```


## 现代std::thread  

cpp11开始，提供了语言级别的`std::thread`类以支持多线程。std::thread构造函数的参数可以是任何lambda表达式，当线程启动时就会执行lambda表达式中的内容。     
异步的目的未必完全是为了跑满多个核从而提升程序执行效率，并行是另一个目的，当迅雷进行下载的同时，你任然能操作它的主界面，这就是并行。   

### std::thread生命周期

1. 创建：当一个std::thread对象被创建时，它启动一个新线程，线程开始执行指定的任务。
2. 连接（Join）：调用join()会阻塞调用该方法的线程（通常是主线程），直到该std::thread对象所管理的线程完成其任务。这是确保线程资源被正确回收的一种方式。
3. 分离（Detach）：调用detach()会将std::thread对象与其管理的线程分离。分离后的线程将独立运行，不再受std::thread对象管理，当它完成时，系统会自动回收其资源。
4. 销毁：std::thread对象被销毁。如果一个线程在被销毁时仍然是可连接的，程序会调用std::terminate()并导致异常终止。
joinable() 的含义

### 线程独立于std::thread

*bool std::thread::joinable`*   
joinable用于检查线程对象是否**持有**一个可操作的线程。它返回一个布尔值，表示线程对象当前是否关联着一个线程。如果返回 true，表示该线程对象持有一个有效的线程，且该线程尚未被 join() 或 detach() 处理。   

```cpp
// 1. 默认构造的线程对象
// 这种情况下，t1.joinable() 返回 false，因为 t1 没有被初始化为持有任何线程。
std::thread t1;

// 2. 持有有效线程的对象
// 这种情况下，t2.joinable() 返回 true，
// 因为 t2 持有一个有效的线程，该线程正在运行或已经运行完毕但未被处理。
std::thread t2(threadFunction);

// 3.调用 join() 后 
// 这种情况下，t2.joinable() 返回 false，因为线程已经被处理并且资源已经被回收 不再被持有
t2.join();

// 4.调用 detach() 后
// 这种情况下，t.joinable() 返回 false，因为线程已经被分离，不再受 t2 持有
t2.detach();
```

### 主线程等待子线程


```cpp
// 如果tTest 持有了一个线程
if (tTest.joinable()) { 
    // 主线程阻塞，阻塞以等待tTest持有的线程退出后继续执行
    tTest.join();       
}
```

### 线程分离  

std::thread 持有并管理线程，std::thread::detach接口使得std::thread与其持有的线程相分离。  
在未分离情况下，当std::thread析构，其所管理的线程也会被释放回收。   
分离后，std::thread析构将不触发线程资源释放,而是线程退出后自己将自己释放。    

但是，无论如何，当进程退出，所有进程下的子线程必然释放。   


### 线程移交，而不可拷贝  

std::thread 不可copy，只能move.   
本质来说，移动移动的是std::thread管理的线程，移交的是线程的管理权而非线程本身。 

```cpp
class ThreadPool{
    std::vector<std::thread> m_pool;
public:
    // .......
    void push_back(std::thread thr){
        m_pool.push_back(std::move(thr));
    }
};

int main() {
    std::thread t([&](){
        std::cout << "do";
    });
    ThreadPool pool_thread;
    // 线程对象只能移动，不能拷贝
    pool_thread .push_back(std::move(t));

    return 0;
}
```


## 异步   


### 构建异步对象 

std库下 std::async 接口接受一个带返回值的lambda表达式，自身返回一个std::future对象。   
lambda的函数体将在另一个线程中默认的自发执行。     

```c++
#include <future>
#include <thread>

std::future<int> fret = std::async([&](){
    std::cout << "Run start ************" << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 阻塞两秒
    std::cout << "Run endl  ************" << std::endl;

    return 404; // 返回值
});
```

### 等待异步   

> 等待线程退出 并获取返回值

`future::get` 接口调用后将会获取异步线程下运行的 函数 的返回值。   
**如果函数还没执行完成呢？** get接口的调用线程将会阻塞，直到函数返回。    
这就是显式的异步等待。   

另一个接口是`future::wait`,其与 get相同，区别在于wait等待不会获取返回值。

```c++

/** 执行结果：
Run start ************
Run endl  ************
async::get(): 404
耗时: 0.999904 秒 
*/
#include <future>
#include <thread>
#include <iostream>

int main() {

    std::future<int> fret = std::async([&](){
        std::cout << "Run start ************" << std::endl;
        std::this_thread::sleep_for(std::chrono::seconds(2));  // 阻塞两秒
        std::cout << "Run endl  ************" << std::endl;

        return 404; // 返回值
    });

    std::this_thread::sleep_for(std::chrono::seconds(1));  // 阻塞一秒 

    auto start = std::chrono::high_resolution_clock::now(); 
    std::cout << "async::get(): " << fret.get() << std::endl; // 此时会触发（2-1=1秒阻塞）
    auto end = std::chrono::high_resolution_clock::now();

    std::chrono::duration<double> elapsed = end - start;
    std::cout << "耗时: " << elapsed.count() << " 秒" << std::endl;

    return 0;
}

```

> 超时时间下的等待

wait与get均会无限等待，可以使用`future_status future::wait_for`设定最长等待时间 

```cpp
/**
// names for timed wait function returns
_EXPORT_STD enum class future_status 
{ 
    ready,      // 完毕
    timeout,    // 放弃了
    deferred    
};
*/

// 尝试等待100ms
fret.wait_for(std::chrono::milliseconds(100));
```

### std::future的懒惰模式   

不直接创建一个子线程运行函数，在创建 std::future对象时，将std::async接口的第一个参数设为`std::launch::deferred`，使得在把对函数体的执行推迟到显式的调用std::future::get后  
这是编程范式的意义下的异步，并非基于多线程的异步。   

### 拆开std::async的包装  

std::async函数做了什么事情，它是如何实现这种异步的？  
`std::promise` 是一个模板类型，其用途在线程间进行安全的值传递。     
std::promise<void>是存在的，它的std::promise::set_value不接受任何值，仅仅用于同步   

```cpp
std::promise<int> pret;
std::thread t ([&](){
    // 执行耗时操作
    std::this_thread::sleep_for(std::chrono::seconds(2));  
        
    // 我的返回值  
    int ret = 404; 
    pret.set_value(ret);
});

std::future<int>  fret = pret.get_future();
```


## 互斥量   

* *线程安全（MT-safe）*  
* *数据竞争（data-race）*

`std::vector`不是线程安全的容器,例如当std::vector需要扩容时，有潜在的多个线程同时对一个std::vector执行free从而引发崩溃。   

### 上锁思想的本质   

上锁的本质在于阻止两个线程同时进入一个代码段，一个代码段就是厕所的坑位，当线程A进去后就把**厕所门**给锁上，解完手后再把厕所门解开。   
厕所门 就是上锁的本质，厕所门是**锁对象**，其有*上锁（lock）*，*未上锁（unlock）*两种状态。当遇到厕所门处于锁定时，线程就会在门前等待，直到门打开。     

几个厕所就要定义几个厕所门，如下，对std::vector<int> Vec_int_对象的操作 我认定为是一个厕所，为这个厕所构造了一个厕所门，即 std::mutex mtx对象。


```cpp
std::vector<int> Vec_int_; // 厕所
std::mutex mtx;            // 厕所门

std::thread t1([&](){   
    for( int i = 0; i < 1000; i++)
        mtx.lock();     // 随手开门
        Vec_int_.push_back(1);
        mtx.unlock();   // 关门
    }
});

std::thread t2([&](){   
    for( int i = 0; i < 1000; i++)
        mtx.lock();         
        Vec_int_.push_back(2);
        mtx.unlock();
    }
});

```

### RAII下的 锁 思想   

*RAII （Resource Acquisition Is Initialization）（资源获取即初始化）*  
RAII是一种重要的C++编程思想，用于管理资源的生命周期，以确保资源在对象的生命周期内得到正确的分配和释放。例如，智能指针。   
RAII的核心思想是将资源的获取与对象的生命周期绑定在一起。当一个对象被创建时，它会自动获取资源；当对象被销毁时，它会自动释放资源。这种方式确保了资源的正确管理，避免了资源泄漏和其他相关问题。   

上锁是否是一种锁资源的获取？    
下锁是否是一种锁资源的释放？   
这就是 锁 ，在RAII思想中的体现。   
  
```cpp

/** std::lock_guard 锁套工具
*/

std::vector<int> Vec_int_; // 厕所
std::mutex mtx;            // 厕所门

std::thread t1([&](){   
    for( int i = 0; i < 1000; i++)
        std::lock_guard grd(mtx);   // 构造函数中 调用std::mutex::lock
        Vec_int_.push_back(1);
    }                               // 离开作用域时析构，析构函数中调用std::mutex::unlock
});

// 也可以用空花括号碰瓷作用域退出
{
    std::lock_guard grd(mtx);   // 构造函数中 调用std::mutex::lock
    Vec_int_.push_back(1);
}


/** std::lock_guard 也存在一个问题 ———— 死板，不析构不解锁，无法手动解锁 
*/
{
    std::lock_guard grd(mtx);   // 构造函数中 调用std::mutex::lock
    int a = 1; 
    Vec_int_.push_back(1);    
}   
// 因为要用作用域碰瓷std::lock_guard的析构函数，我在作用域中创建的对象外部无法访问，有时候就挺烦
// 可否有一个 锁工具 灵活一点，既可以碰瓷析构函数，又可以手动下锁？   有，std::unique_lock 

/** std::unique_lock 锁工具
*/
std::unique_lock<std::mutex> qrd(mtx);   // 构造函数中 调用std::mutex::lock
int a = 1; 
Vec_int_.push_back(1);  
qrd.unlock();       // 可以手动解锁
```


### 无阻塞的尝试上厕锁  

在前面的 锁 对象问题中，std::mutex::lock 接口会持续的等待厕所开门 直到厕所开门。    
如果我不着急，就来**无阻塞的**看一眼厕所开没开门，如果开门了我就进去上厕所，如果没开门 **我也不等待，直接退出** ————`bool std::mutex::try_lock`。   
返回值bool 返回当前厕所门是否开启。   


### 有超时的上厕锁   

介于干等和看一眼之间，有一个**有超时等待**一段时间的上厕锁方式————`std::mutex::try_lock_for`

```cpp
// 厕所门
std::mutex mtx;           

// 尝试等待一段时间
if(mtx.try_lock_for(std::chrono::seconds(2)){
    std::cout << "我排到了";
}else {
    std::cout << "等待超时";
}   
```
 






