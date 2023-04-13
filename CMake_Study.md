## 不引入第三方库

目录结构：

```markdown
./Demo1
    |
    +--- main.cc
    |
    +--- math/
          |
          +--- MathFunctions.cc
          |
          +--- MathFunctions.h
```

需要在项目根目录 Demo1 和 math 目录里各编写一个 CMakeLists.txt 文件。将 math 目录里的文件编译成静态库再由 main 函数调用。

math 目录中的 CMakeLists.txt：

```cmake
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)

# 生成静态链接库MathFunctions
add_library (MathFunctions ${DIR_LIB_SRCS})
```

根目录中的 CMakeLists.txt ：

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目名称信息，同时会自动生成 PROJECT_NAME 变量，使用 ${PROJECT_NAME} 即可访问到 Demo1
project (Demo1)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量，使用 ${DIR_SRCS} 可访问到
aux_source_directory(. DIR_SRCS)

# 添加 math 子目录
add_subdirectory(math)

# 指定生成目标 
add_executable(Demo main.cc)

# 添加链接库
target_link_libraries(Demo MathFunctions)
```

单独命令：

```cmake
# 将从第二个往后的参数保存到 DIR_SRCS 变量中，使用 ${DIR_SRCS} 可以访问到
set(DIR_SRCS
    MathFunctions.cc
    main.cc
)
```

## 引入第三方库

### 目录结构：

```markdown
.
├── CMakeLists.txt
├── main.cpp
└── README.adoc
```

### 本地导入（find_package）

以 boost 为例，MakeLists.txt：

```cmake
cmake_minimum_required(VERSION 3.5)

project (third_party_include)
# 使用库文件系统和系统查找 Boost，找到了，Boost_FOUND 为真，否则为假
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)
# 1.46.1 代表需要库的最低版本；REQUIRED 表示找不到会报错；COMPONENTS 用于检测该库的对应组件是否存在，如果不存在则认为找到的库不满足条件。
if(Boost_FOUND)
    message ("boost found")
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()

# 指定生成目标
add_executable(third_party_include main.cpp)

# 添加链接库
target_link_libraries(third_party_include PRIVATE Boost::filesystem)
```

这里使用 `find_package` 命令来在本地搜索对应的第三方库，Boost 代表需要查询的库名称；1.46.1 代表需要库的最低版本；REQUIRED 表示该库是必须的，如果找不到会报错；COMPONENTS 用于检测该库的对应组件是否存在，如果不存在则认为找到的库不满足条件。

### 外部导入（FetchContent）

以 GoogleTest 库为例：

```cmake
cmake_minimum_required(VERSION 3.14)
project(my_project)

set(CMAKE_CXX_STANDARD 11)

# 引入 FetchContent 模块
include(FetchContent)
# 获取第三方库，可以是一个 URL 或者一个 Git 仓库
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

# 将这个第三方库引入项目
FetchContent_MakeAvailable(googletest)
```
