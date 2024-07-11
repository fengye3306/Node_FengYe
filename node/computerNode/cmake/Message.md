# 新信息 


## find_package 确定对应包名

是Qt?还是qt?是OpenCV?亦或是opencv?你是否也因为这件事头疼过？    

*nodeeditor* 是一个第三方库，我将其编译为动态链接库，cmake项目欲以find_package将其引入。   
```cmake
find_package(nodeeditor REQUIRED)    
```
error！！！*nodeeditor*库未找到。   
你很沮丧,不知所措。     

find_package的调用其实基于*Config.cmake*文件，*nodeeditor* 库编译后形成的config寻址文件名为：*QtNodesConfig.cmake* ，find_package调用时应该截取`QtNodes`这一字符。   

```cmake
find_package(QtNodes REQUIRED)  
```
此时成功寻址。   

下次需要用到find_package 报错头疼时，不妨去看看config文件前缀。

