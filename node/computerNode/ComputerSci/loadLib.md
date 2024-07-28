# lib


## 库接口导出   

> 查询接口是否正常导出

` nm -D ${库名} | grep  ${接口名} `

```sh
# 例如
nm -D ./libalginedd.so | grep createImageProcessing2DPluginInterface
```

> 导出宏&接口导出 示例

```cmake
# CMake添加宏定义
add_definitions(-Dalgined_LIBRARY)
```

```cpp

// 导出宏
#if defined(algined_LIBRARY)
#  if defined(_WIN32)
#    define algined_LIBRARY_EXPORT __declspec(dllexport)
#  else
#    define algined_LIBRARY_EXPORT __attribute__((visibility("default")))
#  endif
#else
#  if defined(_WIN32)
#    define algined_LIBRARY_EXPORT __declspec(dllimport)
#  else
#    define algined_LIBRARY_EXPORT
#  endif
#endif
```

*注意！我遇到了在头文件中同时声明和定义后接口未能正确导出的问题！测试暂时未能发现具体原因，也未能找到相关资料说明，在此记录*

```cpp
// .hpp
extern "C" algined_LIBRARY_EXPORT
std::unique_ptr<LinkSense::ImageProcessing2DPluginInterface> createImageProcessing2DPluginInterface();

// .cpp 
extern "C" algined_LIBRARY_EXPORT
std::unique_ptr<LinkSense::ImageProcessing2DPluginInterface> createImageProcessing2DPluginInterface()
{
    return std::make_unique<LinkSense::Algined::Tool_Algined>();
}
```


