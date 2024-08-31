


## 对网络环境更加宽容的git

```bash
git config --global fetch.retries 500
git config --global fetch.retryDelay 5
git config --global http.postBuffer 524288000
git config --global http.lowSpeedLimit 1000
git config --global http.lowSpeedTime 500000
```

## github秘钥配置

```Link
https://www.cnblogs.com/yessn/p/16295806.html#label4   
```



## git安装子库

```bash

# <repository-url> 项目路径
# <path>           子库安装路径
git submodule add <repository-url> <path>
# eg: git submodule add git@github.com:fengye3306/CloudCompare.git ./3rdparty/CloudCompare
```



## 拷贝子模块

例如CloudCompare 其依赖于其他第三方子仓库，直接进行clone 并不会自动拷贝其依赖项。  

```bash
# clone子模块
git submodule update --init --recursive
```

## 主工程安装子模块

```bash

# <repository-url> 项目路径
# <path>           子库安装路径
git submodule add <repository-url> <path>
# eg: git submodule add git@github.com:fengye3306/CloudCompare.git ./3rdparty/CloudCompare
```

