# Cmake

## Cmake 基础语法

### 构建
> 构建动态链接库

```cmake
    add_library(${PROJECT_NAME} SHARED
        ${header_list}
        ${source_list}
    )
```

### 文件



### 


## Cmake 模块新知
### include作用域互不干扰策略

```CC
# NO_POLICY_SCOPE标识符表示此配置并不影响外部作用域
include(CMakePolicies NO_POLICY_SCOPE)   
```

> 扩展   

* OPTIONAL     
表示被包含的文件是可选的。如果CMake不能找到这个文件，它将不会产生错误，而是继续处理其它命令。
* RESULT_VARIABLE    
这个选项允许你指定一个变量来保存include()命令的返回结果。如果被包含的文件被成功处理，这个变量的值将会被设置为NOTFOUND。
* NO_POLICY_SCOPE     
这个选项表示被包含的文件中的策略设置不应影响到包含它的父级作用域。


### 工程文件的优雅处理

```cc

file( GLOB header_list *.h )				# 所有.h文件
file( GLOB source_list *.cpp )				# 所有.cpp文件
file( GLOB ui_list ui_templates/*.ui )		# 所有UI文件
file( GLOB qrc_list ../qCC/*.qrc )			# 所有资源文件
file( GLOB rc_list ../qCC/*.rc )	    	# 所有.rc文件

# 使用Qt5的uic工具处理.ui文件并生成头文件，
# 将生成的头文件路径保存到generated_ui_list变量中。
qt5_wrap_ui( generated_ui_list ${ui_list} )
# 使用Qt5的rcc工具处理.qrc文件并生成源文件，
# 将生成的源文件路径保存到generated_qrc_list变量中。
qt5_add_resources( generated_qrc_list ${qrc_list} )



# 生成结果文件 
if( MSVC )
	# App icon with MSVC
	set( rc_list images/icon/cc_viewer_icon.rc )

	#to get rid of the (system) console
	add_executable( ${PROJECT_NAME} 
		WIN32 
		${header_list} 
		${source_list} 
		${generated_ui_list} 
		${generated_qrc_list}
		${rc_list}  
		${CMAKE_CURRENT_SOURCE_DIR}/../scripts/windows/qt5.natvis	# 此文件用于在调试时更好的显示qt类型
	)

elseif( APPLE )
	add_executable( ${PROJECT_NAME} 
		MACOSX_BUNDLE 
		${header_list} 
		${source_list} 
		${generated_ui_list} 
		${generated_qrc_list} 
		${rc_list} 
	)
else()
	add_executable( ${PROJECT_NAME} 
		${header_list} 
		${source_list} 
		${generated_ui_list} 
		${generated_qrc_list} 
		${rc_list} 
	)
endif()
```
### Vscode cmake工具配置





### find_package 确定对应包名

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

