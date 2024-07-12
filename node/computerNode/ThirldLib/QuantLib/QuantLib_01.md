# 金融工具


## 金融工具设计思路   


> 设计需求

* **一个工具多种业务**

一个给定的金融工具可能以不同的方式定价（例如，用一个或多个解析公式或数值方法），而不必借助于继承。

* 工具价值计算源于**输入数据**，数据随时间变化，甚至数据在不同时间从不同来源提供   

另一个变化因素是任何单一的市场数据都可以由不同的来源提供。我希望金融工具能够保持与这些数据源的联系，以便在不同的调用中，它们的方法可以获取最新的值并相应地重新计算结果；另外，我希望能够在数据源之间透明地切换，并让金融工具**仅仅将其视为数据值发生的更改**。      

* 预防潜在效率损失   

例如，我们可以通过将其金融工具存储在容器中，定期轮询它们的价值并添加到最终结果中，  
从而及时监控投资组合的价值。在一个简单的实现中，即使对那些输入没有变化的金融工具也会引发重新计算。



> 作为输入数据的市场输入 设计思路   

当任何输入改变时，通过观察者模式通知金融工具对象。显然，金融工具扮演观察者的角色，而输入数据则扮演被观察者的角色。为了在被通知输入发生变化后可以访问新值，观察者需要维护表示输入对象的引用。然而，指针的行为不足以完全描述我们的问题。正如我已经提到的，变化可能不仅源于数据源的值随时间而变化，我们可能还想切换到其他数据源。存储（智能）指针可以让我们访问指向对象的当前值，但是我们的指针副本，对于观察者来说是私有的，不能指向其他不同的对象。因此，我们需要的是指向指针的指针的智能等价物。    
*这个特性在 QuantLib 中以类模板的形式实现，并命名为 Handle。*     

在金融领域的编程库 QuantLib 中，Handle 类扮演了一个非常重要的角色。让我们用一种更通俗易懂的方式来理解这个概念。   
假设你有一个电视遥控器。遥控器的目的就是让你能控制电视，比如改变频道或者调整音量。但是，重要的是，你不需要知道电视是如何工作的，也不需要知道遥控器是如何发送信号给电视的。你只需要知道按下哪个按钮，电视就会有对应的反应。  
在这个例子中，遥控器就像是 Handle 类。它是一个中间人，处理和传递信息。它维护了一个对另一个对象（在这个例子中是电视）的引用，并可以随时切换到另一个对象。当被观察的对象（电视）发生变化时，Handle 会通知所有正在观察的对象（使用遥控器的人）。  
这就是 Handle 类的基本概念：**它是一个智能指针的智能指针。它允许我们不仅可以访问当前链接的对象，还可以在运行时改变链接到的对象**，并且能够通知所有观察者对象发生的变化。这为处理复杂的金融数据提供了很大的便利，例如市场数据，利率或波动率期限结构等。  


> 懒加载的业务类 的设计    

另一个问题是抽象出用于存储和重新计算缓存结果的代码，同时仍然将其留给派生类来实现任何特定的计算。  
*在QuantLib中LazyObject* 对此完成了实现。      
缓存的逻辑很简单。如果当前结果不再有效，我们让派生类执行所需的计算并将新结果标记为最新。如果目前的执行结果是有效的，我们什么也不做。   



## 基于监视者模式的懒加载金融工具     


> 观察者框架概述

```cpp
    /* 观察者
     * 被监视者类对象 信号的提醒对象
     */
    class Observable {
        // 通知所有观察者
        void notifyObservers(){
            // ......
            for (auto* observer : observers_) {
                try {
                    // 发通知
                    observer->update(); 
                } catch (std::exception& e) {
                    //......
        }

        // 我作为被观察者 记下了观察我的 观察者列表 我被触发时直接通知 观察者  
        // typedef std::set<Observer*> set_type;
        set_type observers_;
    }

```

```cpp
    /* 被观察者
     * 一旦它发生了变化，就会通知所有正在观察它的观察者。
     */
    class Observer { 

        // 将当前的观察者注册到一个可观察对象（Observable）上
        registerWith(const ext::shared_ptr<Observable>&)：
        // 从另一个观察者身上拷贝所有被观察对象的绑定状态
        registerWithObservables(const ext::shared_ptr<Observer>&);
        // 注销对一个被观测对象的绑定
        unregisterWith(const ext::shared_ptr<Observable>&)
        // 注销对所有被观测对象的绑定
        unregisterWithAll();
        // 我接受到了被观察者的通知 只更新自己
        virtual void update() = 0：
        // 深度通知 我接受到了被观察者的通知 要求链式传导通知 
        virtual void deepUpdate();
    }
```



```cpp
    /* 懒加载_观察者与被观察者
     * 1. 这个类可以作为观察者监视其他 被观察者
     * 2. 这个类可以被其他类监视
     * 3. 这个类这个对象并不总是立刻响应观察到的变化
        它的"懒惰"体现在它会缓存计算结果，只有在必须的时候才重新计算.
     */
    class LazyObject : 
        public virtual Observable,
        public virtual Observer {
        
        // update 不是直接调用calculate或者recalculate进行计算
        // 而是 直接更改了 calculated_属性的值  
        void update(){
            // ......
            if (calculated_ || alwaysForward_) {
                // 直接更改了 calculated_属性的值
                calculated_ = false;
                if (!frozen_)
                    // 监视传递
                    notifyObservers();
            }
            // ......
        }

        // 锁、解锁_ 本类在作为观察者被触发的时候 
        // 是否触发观察着自己的 观察者类？
        void freeze();
        void unfreeze();

        // 强制计算结果
        recalculate(){
            bool wasFrozen = frozen_;
            // 确保不传递信号的情况下强行调用一次 执行业务
            calculated_ = frozen_ = false;
            try {
                calculate();
            } catch (...) {
                frozen_ = wasFrozen;
                notifyObservers();
            throw;
        }
        frozen_ = wasFrozen;
        notifyObservers();        
        };

        // 执行一次业务
        // 计算结果 如果calculated_ == true 就懒加载
        calculate(){
            if (!calculated_ && !frozen_) {
                // 更新一次
                calculated_ = true;                                 
            try {
                performCalculations();// 真正的业务代码
            } catch (...) {
                calculated_ = false;
                throw;
            }
        }
        };

        // 真正的业务代码
        virtual performCalculations() = 0;
        
    protected:
        // 是否为懒加载的实际开关，懒加载下很多计算不用做的 沿用旧的数据结果
        mutable bool calculated_ = false; 
        // freeze锁
        mutable bool frozen_ = false;
        mutable bool alwaysForward_;
            
    }

    
    // 具体金融工具
    class Instrument : public LazyObject {
    }
```

> 总结：一套懒加载的高性能监视者模式  

在 LazyObject::update 方法中并没有直接调用 LazyObject::calculate 方法。LazyObject::update 方法主要负责在被观察的对象改变时，标记 LazyObject::LazyObject 对象需要重新计算，并向观察自己的观察者发送通知（如果监视传递的开关被开启）。   
在 LazyObject 的设计中，观察者通常在收到通知后，会通过调用 LazyObject::LazyObject 对象的 LazyObject::calculate 方法或者其他类似的方法，进行真正的业务执行。  
所以，calculate 方法的调用通常是在 update 方法之后，由观察者进行的。    
这种设计使得 LazyObject 对象能够*延迟计算*，即只有当结果真正需要的时候才进行计算。这样可以提高效率，特别是当计算成本较高或者结果并不总是需要的情况下。   


另一个值得注意的点是对业务代码的异常处理    
```cpp
inline void LazyObject::calculate() const {
    if (!calculated_ && !frozen_) {
        calculated_ = true;   // prevent infinite recursion in
                                // case of bootstrapping
        try {
            performCalculations();
        } catch (...) {
            // 小细节  
            calculated_ = false;
            throw;
        }
    }
}
```

## 对一个类的执行状态标识,阻止调用冲突   

这个思路在上面小结中未能体现，但我觉得特别棒，下面我要单独记录。    
实现方案说白了，就是UpdateChecker这个内部类一被定义，我的LazyObject类就被占用了！         

```cpp
class LazyObject : public virtual Observable,
                       public virtual Observer {

// ......
private:
    bool updating_ = false;
    class UpdateChecker {  // NOLINT(cppcoreguidelines-special-member-functions)
        LazyObject* subject_;
        public:
        explicit UpdateChecker(LazyObject* subject) : subject_(subject) {
            subject_->updating_ = true;
        }
        ~UpdateChecker() {
            subject_->updating_ = false;
        }
    };                        conda install pandas   
// ......
}


// 使用实例
 inline void LazyObject::update() {
    // 如果是占状态
    if (updating_) {
        // 且定义了异常日志
        #ifdef QL_THROW_IN_CYCLES
        // 我要告知异常日志
        QL_FAIL("recursive notification loop detected; you probably created an object cycle");
        #else
        return;
        #endif
    }

    // 就在这，我只要定义了UpdateChecker checker(this);直接宣布了占用状态
    // This sets updating to true (so the above check breaks the
    // infinite loop if we enter this method recursively) and will
    // set it back to false when we exit this scope, either
    // successfully or because of an exception.
    UpdateChecker checker(this);
    
    // 业务代码
    // forwards notifications only the first time
    if (calculated_ || alwaysForward_) {
        // set to false early
        // 1) to prevent infinite recursion
        // 2) otherways non-lazy observers would be served obsolete
        //    data because of calculated_ being still true
        calculated_ = false;
        // observers don't expect notifications from frozen objects
        if (!frozen_)
            notifyObservers();
            // exiting notifyObservers() calculated_ could be
            // already true because of non-lazy observers
        }
    }
}
```




## 利率互换工具实例   

*/QuantLib/ql/instruments/swap.hpp*   
*掉期(互换)合约*   

```cpp

/*
* 掉期合约
*/
class Swap : public Instrument {

    /*! In most cases, the swap has just two legs and can be
        defined as receiver or payer.

        Its type is usually defined with respect to the leg paying
        a fixed rate; derived swap classes will document any
        exceptions to the rule. 
        ----------------------------------------------------------------
        接收者（Receiver）  指在互换交易中，接收固定支付的一方。
        比如，在一个利率互换交易中，如果一方约定接收固定利率的利息支付，那么这一方就被称为接收者。  
        支付者（Payer）     则是指在互换交易中，支付固定支付的一方。
        同样在一个利率互换交易中，如果一方约定支付固定利率的利息，那么这一方就被称为支付者。

        简单来说 支付浮动利率的是接收者，支付固定利率的是支付者。   
        
    */
    enum Type { 
        Receiver = -1, 
        Payer = 1 
    };   






}
```


