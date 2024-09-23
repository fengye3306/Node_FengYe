


## 增加git对网络环境的宽容

```bash
# 指定在执行 git fetch 操作时，如果遇到网络或其他临时性错误，Git 会重试的次数。
git config --global fetch.retries    500
# 定义在两次重试之间的等待时间，单位为秒。
git config --global fetch.retryDelay 5
# 设置 Git 在通过 HTTP 协议进行 POST 操作时，允许的最大缓冲区大小（以字节为单位）。
git config --global http.postBuffer  524288000
# 定义 Git 在传输过程中认为速度过低的阈值，单位为字节每秒。
git config --global http.lowSpeedLimit 1000
# 定义在传输速度低于 http.lowSpeedLimit 时，Git 等待多长时间（单位为秒）后才中断操作。
git config --global http.lowSpeedTime 500000
```

## github秘钥配置

```Link
https://www.cnblogs.com/yessn/p/16295806.html#label4   
```

## 子工程

> 主工程安装子工程

```bash
## <repository-url> 目标子工程仓库地址
## <path>           目标子工程配置在主工程的路径地址
git submodule add <repository-url> <path>

## eg: 
# git submodule add git@github.com:fengye3306/CloudCompare.git ./3rdparty/CloudCompare
```

> 拷贝&更新子工程

```bash
# 对于默认拷贝（git clone）的一个项目，默认拷贝主工程而不带子工程
# 使用submodule update 更新子模块
git submodule update --init --recursive
```
