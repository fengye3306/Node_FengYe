

## git克隆附带子库

```bash
git submodule update --init --recursive
```

## 对网络环境更加宽容的git

```bash
git config --global fetch.retries 500
git config --global fetch.retryDelay 5
git config --global http.postBuffer 524288000
git config --global http.lowSpeedLimit 1000
git config --global http.lowSpeedTime 500000
```

## github秘钥

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




