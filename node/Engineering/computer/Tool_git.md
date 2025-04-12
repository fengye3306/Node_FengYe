

## 拉取优化

*本模块址在解决git clone 时遇到的问题*  

> 仅克隆最新版本

`git clone --depth 1 https://github.com/xxxxx/xxxxxxx.git`  

当项目过大时，git clone时会出现error: RPC failed; HTTP 504 curl 22 The requested URL returned error: 504 Gateway Time-out的问题，此时我们可以只下载远程仓库中的最新的一个版本，而不下载其他老版本的内容，这样会大大减小存储与传输压力。  


> 增加git对异常的宽容（也许没什么用，主要还是网得好）

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

## 项目子工程

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

## github拉取error

> Failed to connect to github.com port 443

fatal: unable to access 'https://github.com/fengye3306/Node_FengYe.git/': Failed to connect to github.com port 443 after 21052 ms: Could not connect to server

开启翻墙且浏览器打得开github页面,本地拉取代码时发生error，问题本质是git工具没有走翻墙。   

```ssh
# 以实际代理ip:port为准

# 配置socks5代理 
git config --global http.proxy socks5 127.0.0.1:7890
git config --global https.proxy socks5 127.0.0.1:7890
# 配置http代理
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890
```
本质是要使得git走代理，即（127.0.0.1）是使用的代理的主机号，7890是数据转发端口。     


**扩展**   
```ssh
# 查看代理命令
git config --global --get http.proxy
git config --global --get https.proxy  
# 取消代理命令
git config --global --unset http.proxy
git config --global --unset https.proxy
```
