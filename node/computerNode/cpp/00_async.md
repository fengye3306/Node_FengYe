
## 协程优势  

协程在io密集的场景非常好用，尤其是用于实现网络框架上，例如网络框架总是遇到的一个问题是，网络的io操作会阻塞。  

如果是协程托管，网络框架进行读取时，没有读取到数也会立刻返回，直到下一次轮询。


## 自己实现一个async框架

> 协程操作系统级别接口,Linux下ucontext包   

linux ucontext族函数的原理及使用  
`https://blog.csdn.net/qq_44443986/article/details/117739157`  

* `getcontext`保存上下文，将当前运行到的寄存器信息保存到ucontext_t结构体中。  
* `setcontext`恢复上下文，将ucontext_t结构体变量中的上下文信息重新恢复到cpu中执行。   
* `makecontext`修改上下文，给ucontext_t上下文指定一个程序入口函数，让程序从该入口函数开始执行。   
* `swapcontext`切换上下文，将一个要执行的上下文恢复到cpu中。  
 
