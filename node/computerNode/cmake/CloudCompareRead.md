
*阅读CloudCompare源码，解析CloudCompare开源工程Cmake配置方案*



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


### 项目文件的优雅处理

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


### 链接器标志（Linker flags）    

*set_target_properties 设置连接器标志，其有很多用途*    
源码使用微软的Visual C++编译器编译${PROJECT_NAME}项目时，添加一个链接器标志"/MANIFEST:NO"。这个标志告诉链接器不生成manifest文件。    
```cmake
# set_target_properties 
if (WIN32)
	if (MSVC)
		set_target_properties( ${PROJECT_NAME} PROPERTIES LINK_FLAGS " /MANIFEST:NO" )
	endif()
endif()
```

> 扩展_连接器控制更多作用   

1. 控制符号解析：
链接器标志可以改变链接器的符号解析方式。例如，-l标志可以改变链接器搜索库的顺序   

2. 控制优化：  
链接器标志可以影响链接器的优化行为。例如，-flto标志启用链接时优化（LTO），-gc-sections标志可以让链接器去除未使用的代码段。

3. 控制输出：  
链接器标志可以影响链接器的输出。例如，-g标志可以让链接器生成调试信息，-Map标志可以让链接器生成链接映射。   


4. 控制链接过程：  
链接器标志可以影响链接过程。例如，-Wl,--no-undefined标志可以让链接器在遇到未解析的符号时停止，-Wl,--verbose标志可以让链接器显示链接过程的详细信息。  

