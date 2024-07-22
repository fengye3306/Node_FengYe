# 多线程  

C语言由POSIX标准下`pthread`以提供多线程支持。   

```Msg
POSIX（可移植操作系统接口，Portable Operating System Interface）标准是一组针对操作系统的接口标准，由IEEE（电气和电子工程师协会）制定。POSIX标准旨在保证不同操作系统之间的可移植性，使得应用程序能够在不同的UNIX系统和类UNIX系统（如Linux、macOS）上运行而无需进行大量修改。POSIX标准涵盖了文件系统操作、进程管理、线程管理、信号处理、网络编程等多个方面。

pthread（POSIX threads，即POSIX线程）是POSIX标准中的一个重要部分，用于在程序中实现多线程编程。pthread提供了一组API（应用程序编程接口），使得开发者可以创建、管理和同步线程。通过使用pthread，开发者可以更高效地利用多核处理器的性能，编写并发和并行的程序。
```
## 线程生命周期

1. 创建：当一个std::thread对象被创建时，它启动一个新线程，线程开始执行指定的任务。
2. 连接（Join）：调用join()会阻塞调用该方法的线程（通常是主线程），直到该std::thread对象所管理的线程完成其任务。这是确保线程资源被正确回收的一种方式。
3. 分离（Detach）：调用detach()会将std::thread对象与其管理的线程分离。分离后的线程将独立运行，不再受std::thread对象管理，当它完成时，系统会自动回收其资源。
4. 销毁：std::thread对象被销毁。如果一个线程在被销毁时仍然是可连接的，程序会调用std::terminate()并导致异常终止。
joinable() 的含义

## 现代std::thread  

cpp11开始，提供了语言级别的`std::thread`类以支持多线程。std::thread构造函数的参数可以是任何lambda表达式，当线程启动时就会执行lambda表达式中的内容。   



### bool std::thread::joinable  

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
### bool std::thread::joinable  



```cpp
#include <iostream>
#include <thread>

int main() {
    std::thread tTest(
        []() {
            std::cout << "thread at: " 
                << std::this_thread::get_id() << std::endl;
        }
    );
    // 打印主线程的ID
    std::cout << "main thread at: " 
        << std::this_thread::get_id() << std::endl;

    // 确保在main函数返回之前处理线程
    if (tTest.joinable()) {
        tTest.join();
    }

    return 0;
}
```
