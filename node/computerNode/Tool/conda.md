# Conda

## 安装python解释器   

```shell

# conda create -n ${环境名} python=${python版本}
conda create -n myenv python=3.8
```

## 环境列表

*带星号为默认环境*
```shell
conda info --envs
```


## 激活环境   

```shell
# conda activate ${环境名}
conda activate myenv
```

## 当前解释器路径  

```shell
which python
```

## 安装库  

> 库安装

```shell
# conda install ${库名}
conda install pandas   
```

> 环境库列表

```shell
conda list
```




## Conda环境打包

```sh
# 生成环境
# 配置的是当前conda环境
# 输出路径为当前路径
conda list --export > requirements.txt   

# 安装环境
conda install --file requirements.txt
```
