# CMake 实践

[TOC]



## 1. 初试

```cmake
PROJECT (HELLO)
SET(SRC_LIST main.c)
MESSAGE(STATUS "This is BINARY dir " ${HELLO_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir "${HELLO_SOURCE_DIR})
ADD_EXECUTABLE(hello ${SRC_LIST})
```

### 1.1 相关语法

```cmake
PROJECT(projectname [CXX] [C] [Java])
- 定义工程名称，默认支持所有语言
- 该指令会隐式定义<projectname>_BINARY_DIR 和 <projectname>_SOURCE_DIR两个变量

SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
- 设置变量，现阶段仅显式设置变量

MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display" ...)
- 用于向终端输出用户定义的信息

ADD_EXECUTABLE(hello ${SRC_LIST})
- 定义工程会生成名为hello的可执行文件 后面是相关源文件(source列表)

ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
- 用于向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置。

INSTALL(TARGETS targets...)
	[[ARCHIVE|LIBRARY|RUNTIME]	[DESTINATION <dir>][PERMISSIONS permissions][CONFIGURATIONS] [Debug|Release|...] [COMPONENT <component>][OPTIONAL][...]]
- 用于安装目标文件，ARCHIVE|LIBRARY|RUNTIME 三个参数分别指静态库，动态库和可执行目标二进制。
- 安装指令还可以用于一般文件，非目标文件可执行程序和目录。

ADD_LIBRARY(libname [SHARED|STATIC|MODULE] [EXCLUDE_FROM_ALL] source1 source2 ... sourceN)
- 
```

- 指令大小写无关，参数和变量大小写相关。但推荐大写指令
- make clean 清理构建结果，无法使用make distclean 来清理中间文件
- 如果工程中存在多个目录，需要确保每个要管理的目录都存在一个CMakeLists.txt
- DESTINATION 中的路径如果以'/'开头则是绝对路径，不以'/'开头则路径前加上${CMAKE_INSTALL_PREFIX}
- 

### 1.2 部分指令

该部分是一些代码的实例，以作提示

``` cmake
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)	%PROJECT_BINARY_DIR指的是编译的发生的当前目录
- 指定目标二进制目录
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
- 指定中间二进制目录
CMAKE_INSTALL_PREFIX 
- 变量类似于 configure–脚本的prefix
```



### 1.3 工程目录

- src: 源文件
- build:编译工程的文件
- bin：目标二进制文件
- lib：中间二进制文件

### 1.x 坑

- cmake 引用定义的变量要加 ${变量名}