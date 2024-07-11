# 安装


## 环境变量   

* `CMAKE_INSTALL_MODE`   
可以直接更改安装规则，将原本的物理拷贝替换为符号链接（symlinks），使用此变量需谨慎   
* `CMAKE_INSTALL_PREFIX`   
安装路径
* `CMAKE_INSTALL_DEFAULT_COMPONENT_NAME`   
默认组件   


## install()   

install 指令用于安装   

### Introduction   

* `DESTINATION <dir>`    
用于设置安装路径，使用相对路径时，路径起始点为`CMAKE_INSTALL_PREFIX`。   

* `PERMISSIONS <permission>...`    
设置指定安装文件权限，例如OWNER_READ（所有者读权限），OWNER_WRITE（所有者写权限......

* `CONFIGURATIONS <config>...`   
可以仅在构建Release配置时，将your_target可执行文件安装到bin目录中。其他构建配置（例如Debug）不会执行这个安装命令  

* `COMPONENT <component>`     

组件安装，用户未必需要一次用到所有的组件，将一部分库归于某个特定模块。OpenCV有很多模块，如“core”、“imgproc”、“highgui”等就是使用这个安装规则。 当你没有特别指定，文件就会被安装到一个名叫"Unspecified"的默认组件`CMAKE_INSTALL_DEFAULT_COMPONENT_NAME`中。   

* `EXCLUDE_FROM_ALL`  
于全局安装`make install`中排除对其的安装，有些用于生产的工具库其不需要在打包发布中进行安装。如果要对此库进行安装则需要单独调用`make install ${name}`进行精细安装。   

* `RENAME <name>`  
单个文件安装时，对文件进行重命名，对于文件列表调用时引入此形参直接报错。    


* `OPTIONAL`  
当文件不存在时是否报错，添加此标签则不会产生报错      


### Signatures    

* `TARGETS <target>... [EXPORT <export-name>]`   
被复制的文件列表参数 ,对于常规可执行文件、静态库和动态库 DESTINATION参数不是必须的，cmake将自动引入到bin、lib、include。    

* `[RUNTIME_DEPENDENCIES <arg>...|RUNTIME_DEPENDENCY_SET <set-name>]`    
生成Config.cmake文件,通过此cmake文件可以寻址 `TARGETS`目标文件，目标路径是绝对路径，路径受`DESTINATION`影响.   











## 安装实例





