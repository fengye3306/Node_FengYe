# 对象池   

* 池化技术特点共同特点：提前创建资源，以备不时之需。   
分配内存，创建进程、线程都设计到用户态到内核态的转换，频繁申请，销毁是对性能的空耗。   

* 对象池实现本质在于：  
申请一个大额内存空间，再把大空间切为小空间。   
当需要使用时候直接分配使用，不再需要额外向系统申请内存空间，使用过后放回池子中。   
