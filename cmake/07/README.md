[TOC]



## 1. 区分 debug 和 release 模式

### 1. 注意

- Debug 和 Release 必须在 **不同的目录** 执行 cmake
- 否则每次当 **切换模式** 时，必须把编译生成的中间文件 **全部删掉**

### 2. 在 CMakeLists.txt 中根据 `CMAKE_BUILD_TYPE 宏定义` 来决定走什么分支

```cmake
IF (CMAKE_BUILD_TYPE MATCHES "debug")
  MESSAGE(STATUS "=======> Debug")
ELSEIF (CMAKE_BUILD_TYPE MATCHES "release")
  MESSAGE(STATUS "=======> Release")
ELSEIF (CMAKE_BUILD_TYPE MATCHES "dev")
  MESSAGE(STATUS "=======> test")
ENDIF()
```

### 3. cmake 时指定 ==release 宏定义==

```
->  ls
CMakeLists.txt
->
->  mkdir release
->  cd release/
```

```
->  cmake -DCMAKE_BUILD_TYPE=release ../
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- =======> Release
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/cmake/demo2/release
->
```

### 4. cmake 时指定 ==debug 宏定义==

```
->  ls
CMakeLists.txt
->
->  mkdir debug
->  cd debug/
```

```
->  cmake -DCMAKE_BUILD_TYPE=debug ../
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- =======> Debug
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/cmake/demo2/debug
->
```

输出 `=======> Debug`



## 2. ADD_DEFINITIONS() 添加 `gcc -D宏定义`

### 1. 目录结构

```
->  ls
CMakeLists.txt	main.c
->  tree
.
├── CMakeLists.txt
└── main.c

0 directories, 2 files
->
```

### 2. main.c 按照【宏定义】条件编译

```c
#include <stdio.h>

int main()
{
#ifdef DEBUG
	printf("DEBUG\n");
#elif RELEASE
	printf("RELEASE\n");
#elif DEV
	printf("DEV\n");
#endif
}
```

### 3. CMakeLists.txt 添加 DEBUG 宏定义

```cmake
# 1. CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 2. 项目的名字
PROJECT(HELLO)

# 3. 项目依赖的所有c源文件
SET(SRC_LIST main.c)

# 4. 给C代码设置 -DDEBUG 宏定义，让 main.c 走 DEBUG 逻辑
ADD_DEFINITIONS(-DDEBUG)

# 5. 编译链接所有c文件，生成可执行的二进制程序文件a.out
ADD_EXECUTABLE(a.out ${SRC_LIST})
```

### 4. 新建【build 目录】下进行 cmake、make、运行可执行程序

```
->  ls
CMakeLists.txt	main.c
->  mkdir build
->  cd build/
```

```
->  cmake ../
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/build
```

```
->  make
Scanning dependencies of target a.out
[ 50%] Building C object CMakeFiles/a.out.dir/main.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
```

```
->  ./a.out
DEBUG
```

### 5. 修改 CMakeLists.txt 添加 RELEASE 宏定义

```cmake
# 1. CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 2. 项目的名字
PROJECT(HELLO)

# 3. 项目依赖的所有c源文件
SET(SRC_LIST main.c)

# 4. 直接修改 CMAKE_C_FLAGS变量 来代替 ADD_DEFINITIONS() 内置命令
SET(CMAKE_C_FLAGS -DRELEASE)

# 5. 编译链接所有c文件，生成可执行的二进制程序文件a.out
ADD_EXECUTABLE(a.out ${SRC_LIST})
```

### 6. 删除之前的 build 目录，再新建【build 目录】下进行 cmake、make、运行可执行程序

```
->  ls
CMakeLists.txt	main.c
->  mkdir build
->  cd build/
```

```
->  cmake ../
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/build
```

```
->  make
Scanning dependencies of target a.out
[ 50%] Building C object CMakeFiles/a.out.dir/main.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
```

```
->  ./a.out
RELEASE
->
```

- 与之前调用 **ADD_DEFINITIONS()** 的效果一致



## 3. ADD_DEFINITIONS() 添加 `g++ -D宏定义`

**CMAKE_CXX_FLAGS** 则是设置C++的参数的变量



## 4. ADD_DEFINITIONS() 添加 `g++ -std=c++11`

```cmake
add_definitions("-Wall -std=c++11")
```

