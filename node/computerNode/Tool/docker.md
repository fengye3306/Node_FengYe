# Docker

> Docker的性能优势  

对比与虚拟机，Docker更节约性能，虚拟机需要模拟整个环境，docker仅仅模拟运行环境。  

> 重要概念：镜像、容器

* `镜像` ：环境打包形成的安装包就叫镜像，可以被一键安装 
* `容器` ：容器是镜像被安装后的状态，每个容器之间都是隔离的独立的



## Docker安装  

桌面版带图形化界面、服务器版只带命令行。   

> Ubuntu 

```sh
sudo apt update
sudo apt install docker.io docker-compose

# 验证
docker -v
```


## Docker 用户组

```sh

# 运行测试当前用户是否有Docker权限
docker run hello-world

# 将当前用户添加到docker 组中
sudo usermod -aG docker $USER

# 刷新用户组
newgrp docker
```

> 什么是Dokcer用户组  

在 Unix 和 Linux 系统中，组是一种机制，用于管理用户对系统资源的访问权限。  
每个文件或进程都有一个所有者（用户）和一个组，以及相应的权限设置。Docker 通过 Unix 套接字 (/var/run/docker.sock) 提供与 Docker 守护进程通信的接口。  
默认情况下，这个套接字只允许 root 用户和 docker 组的成员进行访问。  

默认情况下，只有 root 用户可以访问 Docker 守护进程。这意味着每次运行 Docker 命令都需要使用 sudo。为了简化操作并提高安全性，可以将普通用户添加到 docker 组，这样这些用户就可以不使用 sudo 运行 Docker 命令。  




## Docker 镜像

```sh
# 查看环境中Docker镜像列表
docker images
```








