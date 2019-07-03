[TOC]



## 1. cmake 构建 简单的 C 项目

### 1. 项目目录结构

```
->  pwd
/home/xzh/share/makefile/cmake/demo1
->  tree
.
├── CMakeLists.txt
├── main.c
└── tool.c

0 directories, 3 files
->
```

### 2. 所有的C文件

#### main.c

```c
#include <stdio.h>

extern void run();

int main()
{
	run();
	return 0;
}
```

#### tool.c

```c
#include <stdio.h>
void run()
{
	printf("--- run ---\n");
}
```

### 3. CMakeLists.txt 

```makefile
# 1. CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 2. 随意自己定义的【项目名字】
PROJECT(HELLO)

# 3. 项目所有依赖的【C 源文件】
SET(SRC_LIST main.c tool.c)

# 4. 打印与项目名相关内置变量
MESSAGE(STATUS "==> HELLO Binary dir = " ${HELLO_BINARY_DIR}) # binary dir
MESSAGE(STATUS "==> HELLO Source dir = " ${HELLO_SOURCE_DIR}) # source dir

# 5. 打印与项目相关的内置变量
## project binary dir
MESSAGE(STATUS "==> PROJECT Source dir = " ${PROJECT_BINARY_DIR})
## project source dir
MESSAGE(STATUS "==> PROJECT Source dir = " ${PROJECT_SOURCE_DIR})

# 6. 链接所有 目标文件，生成可执行的二进制程序文件 a.out
ADD_EXECUTABLE(a.out ${SRC_LIST})
```

### 4. cmake 解析 CMakeLists.txt

```
->  cmake .
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
-- ==> HELLO Binary dir = /home/xzh/share/makefile/cmake/demo1
-- ==> HELLO Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/cmake/demo1
->
```

在cmake文件中的输出

```
-- ==> HELLO Binary dir = /home/xzh/share/makefile/cmake/demo1
-- ==> HELLO Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
```

- 如上四个变量都指向【同一个目录】
- 但是一旦 **修改了项目名** 之后，就必须修改 **HELLO_** 前缀名
- 所以一成不变就使用 **PROJECT_** 前缀的内置变量

### 5. 目录下生成 Makefile

查看生成的所有文件

```
->  ls -l
total 36
-rw-rw-r-- 1 xzh xzh 11530 1月  18 20:26 CMakeCache.txt
drwxrwxr-x 5 xzh xzh  4096 1月  18 20:26 CMakeFiles
-rw-rw-r-- 1 xzh xzh  1371 1月  18 20:26 cmake_install.cmake
-rw-rw-r-- 1 xzh xzh   460 1月  18 20:26 CMakeLists.txt
-rw-rw-r-- 1 xzh xzh    65 1月  18 20:22 main.c
-rw-rw-r-- 1 xzh xzh  4698 1月  18 20:26 Makefile
->
```

生成了**Makefile** 文件。

### 6. 再执行 make 解析 Makefile 开始构建 C 源码 

```makefile
->  ls
CMakeCache.txt	CMakeFiles  cmake_install.cmake  CMakeLists.txt  main.c  Makefile  tool.c
->
```

```
->  make
Scanning dependencies of target a.out
[ 33%] Building C object CMakeFiles/a.out.dir/main.c.o
[ 66%] Building C object CMakeFiles/a.out.dir/tool.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
->
```

- 可以看到 **Built target a.out** 成功
- 以使用 **make VERBOSE=1** 查看make详细的构建过程

### 7. 运行编译链接生成的二进制文件

查看编译链接生成的可执行程序App

```
->  ls
a.out  CMakeCache.txt  CMakeFiles  cmake_install.cmake	CMakeLists.txt	main.c	Makefile  tool.c
->
->  ./a.out
--- run ---
->
```



## 2. 支持生成 静态库 与 动态库

### 1. XZHNetwork 项目 目录结构

```
->  pwd
/home/xzh/share/XZHNetwork
->  tree
.
├── CMakeLists.txt
├── include
│   ├── add.h
│   └── sub.h
├── src
│   ├── add
│   │   └── add.c
│   ├── CMakeLists.txt
│   └── sub
│       └── sub.c
└── tests
    ├── CMakeLists.txt
    ├── test_add.c
    └── test_sub.c

5 directories, 9 files
->
```

- **build**：
  - 自己新建的目录，为了构建时，不污染源码主目录
  - 在这个文件夹下执行**【cmake ..】**执行**上级根目录**下的**CMakeLists.txt**，在此目录下生成**Makefile**
  - **build/lib**: 生成的库文件 
  - **build/bin**: 一般放生成的可执行文件 
- **include**：库文件对应的**头文件 xxx.h**
- **src**：当前项目所有的源代码的存放目录
- **tests**：测试代码的存放目录
- **CMakeLists.txt**: cmake的配置文件

测试编译 tests 目录/main.c

```
->  ls
build  CMakeLists.txt  include	src  tests
->  cd tests/
->  gcc test_add.c -I../include ../src/add/*.c
->  ./a.out
10+1 = 11
->
->  gcc test_sub.c -I../include ../src/sub/*.c
->  ./a.out
10-1 = 9
->
```

- 注意不能使用 `-L../src/add/*.c 或 -L./src/sub/*.c`
- 因为 gcc **-L** 选项是指定 **静态库/动态库** 文件的 **路径** 的，并 **不是** 指定 **C文件** 的路径

### 2. 顶层 XZHNetwork/CMakeLists.txt

```cmake
# 1、CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 2、创建一个项目
project (XZHNetwork)

# 3、设置【xxx.h 头文件】include 搜索路径
INCLUDE_DIRECTORIES(
  ${PROJECT_SOURCE_DIR}/include
  /usr/local/include
  /usr/include
)

# 4、设置【lib 库文件】link(链接) 搜索路径
LINK_DIRECTORIES(
  ${PROJECT_BINARY_DIR}/lib
  /usr/local/lib
  /usr/lib
  /lib
)

# 5. 添加所有【编译】的【C源文件目录】
ADD_SUBDIRECTORY(src)
ADD_SUBDIRECTORY(tests)
```

- 指定各种文件的 **搜索路径**
- 注意：**PROJECT_BINARY_DIR**表示当前执行**cmake**命令**所在目录**的**全路径**

### 3. XZHNetwork/src/CMakeLists.txt

```cmake
# 项目信息(模块下的CMakeLists.txt不用写)
## project (XZHNetwork)

# 1、设置变量，表示所有的C源文件路径
SET(XZHNetwork_SRCS
  add/add.c
  sub/sub.c
)

################## 2、编译生成动态库 ###################
# 2.1 ADD_LIBRARY(target名字 SHARED 所有的C文件)
ADD_LIBRARY(XZHNetwork SHARED ${XZHNetwork_SRCS})

# 2.2 生成soname版本管理的动态库
SET_TARGET_PROPERTIES(XZHNetwork PROPERTIES VERSION 1.2 SOVERSION 1)

# 2.3 生成动态库后清除掉中间文件
SET_TARGET_PROPERTIES(XZHNetwork PROPERTIES CLEAN_DIRECT_OUTPUT 1)

################## 3、编译生成静态库 ####################
# 3.1 ADD_LIBRARY(target名字 STATIC 所有的C文件)
ADD_LIBRARY(XZHNetwork_static STATIC ${XZHNetwork_SRCS})

# 3.2 设置输出的静态库的文件名
# （否则静态库的名字与target的名字一样为XZHNetwork_static）
SET_TARGET_PROPERTIES(XZHNetwork_static PROPERTIES OUTPUT_NAME "XZHNetwork")

# 3.3 生成静态库后清除掉中间文件
SET_TARGET_PROPERTIES(XZHNetwork_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

############### 4、设置编译生成的【库文件】存放目录 ##################
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
```

- 添加编译链接生成**库文件**、**二进制可执行文件**的任务
- 指定库文件的输出目录路径

其他的设置

![Snip20180119_2](Snip20180119_2.png)

### 4. XZHNetwork/tests/CMakeLists.txt

```cmake
################# 1、test1 可执行程序 #################
# 生成可执行程序Test1
ADD_EXECUTABLE(Test1 test_add.c)

# 链接libXZHNetwork.so、libc.so
TARGET_LINK_LIBRARIES(Test1 XZHNetwork;c)

################ 2、test2 可执行程序#################
# 生成可执行程序Test2
ADD_EXECUTABLE(Test2 test_sub.c)

# 链接libXZHNetwork.so、libc.so
TARGET_LINK_LIBRARIES(Test2 XZHNetwork;c)

###### 3、设置编译生成的【可执行程序】存放目录 ######
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
```

### 5. XZHNetwork/build 目录下，执行 cmake

```
->  pwd
/home/xzh/share/XZHNetwork
->  ls
CMakeLists.txt	include  src  tests
->  mkdir build
->  cd build/
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
-- Build files have been written to: /home/xzh/share/XZHNetwork/build
->
```

 查看**build**目录下**cmake**生成的文件

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls -l
total 44
drwxrwxr-x 2 xzh xzh  4096 1月  19 09:48 bin
-rw-rw-r-- 1 xzh xzh 11666 1月  19 09:48 CMakeCache.txt
drwxrwxr-x 4 xzh xzh  4096 1月  19 09:48 CMakeFiles
-rw-rw-r-- 1 xzh xzh  1608 1月  19 09:48 cmake_install.cmake
drwxrwxr-x 2 xzh xzh  4096 1月  19 09:48 lib
-rw-rw-r-- 1 xzh xzh  5462 1月  19 09:48 Makefile
drwxrwxr-x 3 xzh xzh  4096 1月  19 09:48 src
drwxrwxr-x 3 xzh xzh  4096 1月  19 09:48 tests
->
```

- 1、多出来几个目录
  - lib
  - bin
  - tests
- 2、生成了**Makefile**文件
- 3、所有的cmake中间文件都只会在build目录下生成，而不会污染项目的根目录

### 6. XZHNetwork/build 目录下 make

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  make
Scanning dependencies of target XZHNetwork_static
[ 10%] Building C object src/CMakeFiles/XZHNetwork_static.dir/add/add.c.o
[ 20%] Building C object src/CMakeFiles/XZHNetwork_static.dir/sub/sub.c.o
[ 30%] Linking C static library ../lib/libXZHNetwork.a
[ 30%] Built target XZHNetwork_static
Scanning dependencies of target XZHNetwork
[ 40%] Building C object src/CMakeFiles/XZHNetwork.dir/add/add.c.o
[ 50%] Building C object src/CMakeFiles/XZHNetwork.dir/sub/sub.c.o
[ 60%] Linking C shared library ../lib/libXZHNetwork.so
[ 60%] Built target XZHNetwork
Scanning dependencies of target Test2
[ 70%] Building C object tests/CMakeFiles/Test2.dir/test_sub.c.o
[ 80%] Linking C executable ../bin/Test2
[ 80%] Built target Test2
Scanning dependencies of target Test1
[ 90%] Building C object tests/CMakeFiles/Test1.dir/test_add.c.o
[100%] Linking C executable ../bin/Test1
[100%] Built target Test1
->
```

### 7. 查看 XZHNetwork/build/lib 下生成的库文件

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  cd lib/
->  ls -l
total 12
-rw-rw-r-- 1 xzh xzh 1932 1月  19 09:49 libXZHNetwork.a
lrwxrwxrwx 1 xzh xzh   18 1月  19 09:49 libXZHNetwork.so -> libXZHNetwork.so.1
lrwxrwxrwx 1 xzh xzh   20 1月  19 09:49 libXZHNetwork.so.1 -> libXZHNetwork.so.1.2
-rwxrwxr-x 1 xzh xzh 6920 1月  19 09:49 libXZHNetwork.so.1.2
->
->  file libXZHNetwork.a
libXZHNetwork.a: current ar archive
->
->  file libXZHNetwork.so.1.2
libXZHNetwork.so.1.2: ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, BuildID[sha1]=741c3e4ab73102873f70ac1c353add52db06ad8c, not stripped
->
```

- 1、静态库：libXZHNetwork.a
- 2、动态库：libXZHNetwork.so.1.2

其中动态库**lib**目录中使用了**soname**机制

```
->  pwd
/home/xzh/share/XZHNetwork/build/lib
->  ls -l
total 12
-rw-rw-r-- 1 xzh xzh 1932 1月  19 14:33 libXZHNetwork.a
lrwxrwxrwx 1 xzh xzh   18 1月  19 14:33 libXZHNetwork.so -> libXZHNetwork.so.1
lrwxrwxrwx 1 xzh xzh   20 1月  19 14:33 libXZHNetwork.so.1 -> libXZHNetwork.so.1.2
-rwxrwxr-x 1 xzh xzh 6920 1月  19 14:33 libXZHNetwork.so.1.2
->
```

- libXZHNetwork.so 是**软链接** => 顶层对外用于链接动态库的链接 => 大版本
- libXZHNetwork.so.1 也是**软链接** => 大版本软链接指向的小版本软链接 => 小版本
- libXZHNetwork.so.1.2 才是真正的**动态库文件** =>具体版本

### 8. 执行 XZHNetwork/build/bin 下生成的可执行程序

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  cd bin/
->  ls -l
total 16
-rwxrwxr-x 1 xzh xzh 7372 1月  19 09:49 Test1
-rwxrwxr-x 1 xzh xzh 7372 1月  19 09:49 Test2
->
->  ./Test1
10+1 = 11
->  ./Test2
10-1 = 9
->
```

查看下可执行程序Test1、Test2库的依赖关系

```shell
->  pwd
/home/xzh/share/XZHNetwork/build/bin
->  ls -l
total 16
-rwxrwxr-x 1 xzh xzh 7372 1月  19 09:49 Test1
-rwxrwxr-x 1 xzh xzh 7372 1月  19 09:49 Test2
->  ldd Test1
	linux-gate.so.1 =>  (0xb7fa6000)
	libXZHNetwork.so.1 => /home/xzh/share/XZHNetwork/build/lib/libXZHNetwork.so.1 (0xb7f9e000)
	libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb7dce000)
	/lib/ld-linux.so.2 (0xb7fa8000)
->  ldd Test2
	linux-gate.so.1 =>  (0xb7f71000)
	libXZHNetwork.so.1 => /home/xzh/share/XZHNetwork/build/lib/libXZHNetwork.so.1 (0xb7f69000)
	libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb7d99000)
	/lib/ld-linux.so.2 (0xb7f73000)
->
```

可以看到依赖生成的动态库**libXZHNetwork.so.1 => /home/xzh/share/XZHNetwork/build/lib/libXZHNetwork.so.1**。

查看可执行程序Test1、Test2库中对依赖的**add、sub**符号：

```
->  readelf -s Test1 | grep add
     2: 00000000     0 FUNC    GLOBAL DEFAULT  UND add
    37: 00000000     0 FILE    LOCAL  DEFAULT  ABS test_add.c
    53: 00000000     0 FUNC    GLOBAL DEFAULT  UND add
->
->  readelf -s Test2 | grep sub
     7: 00000000     0 FUNC    GLOBAL DEFAULT  UND sub
    37: 00000000     0 FILE    LOCAL  DEFAULT  ABS test_sub.c
    68: 00000000     0 FUNC    GLOBAL DEFAULT  UND sub
->
```

可以看到对于**add、sub**符号的**Ndx**都是**UND(undefine)**，也就是**外部的符号**，说明**链接的是动态库**，而不是静态库。



## 3. make install 安装

### 1. cmake 指定执行 make install 时的 ==拷贝到的路径==

- 由 `-D CMAKE_INSTALL_PREFIX=` 参数指定

```make
cmake -D CMAKE_INSTALL_PREFIX=/Users/xiongzenghui/Desktop/XZHNetworkBuild ../lib
```

### 2. 安装：1)可执行程序 2)静态库 3)动态库

```cmake
INSTALL(
  # 列举出三个需要进行安装的文件(target) myrun、mylib、mystaticlib
  # - 1) target myrun => 可执行程序文件
  # - 2) target mylib => 动态共享库文件
  # - 3) target mystaticlib => 静态库文件
  TARGETS myrun mylib mystaticlib	

  # 二进制可执行程序，拷贝到 {CMAKE_INSTALL_PREFIX}/bin 目录
  RUNTIME DESTINATION bin

  # 动态库，拷贝到 {CMAKE_INSTALL_PREFIX}/lib 目录
  LIBRARY DESTINATION lib

  # 静态库，拷贝到 {CMAKE_INSTALL_PREFIX}/libstatic 目录
  ARCHIVE DESTINATION libstatic	
）
```

### 3. 安装 ==普通== 文件

将文件 **COPYRIGHT、README** 两个文件，拷贝到目录 **{CMAKE_INSTALL_PREFIX}/doc** 下

```cmake
INSTALL(FILES COPYRIGHT README DESTINATION doc)
```

### 4. 安装 ==可执行/脚本== 文件

```cmake
INSTALL(PROGRAMS runhello.sh DESTINATION bin)
```

将 **runhello.sh** 到目录 **{CMAKE_INSTALL_PREFIX}/bin** 下

### 5. 安装 ==目录== 文件

```cmake
INSTALL( DIRECTORY 
  # - 1) 将【icons目录】安装到 {CMAKE_INSTALL_PREFIX}/examples/script
  # - 2) 将【scripts/中的内容】安装到{CMAKE_INSTALL_PREFIX}/examples/script
  icons scripts/ DESTINATION examples/script

  # 不包含目录名为 CVS 的目录
  PATTERN "CVS" EXCLUDE

  # 对于 scripts/ 目录下所有的文件分配
  # - 1) OWNER_EXECUTE  拥有者、可执行
  # - 2) OWNER_WRITE 拥有者、可写
  # - 3) OWNER_READ 拥有者、可读
  # - 4) GROUP_EXECUTE 组内、可执行
  # - 5) GROUP_READ 组内、可读
  PATTERN "scripts/*"
  PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ
  GROUP_EXECUTE GROUP_READ
)
```



## 4. make install 综合示例

### 1. 目录结构

```
->  ls
CMakeLists.txt	doc  examples  include	sh  src  tests
->  tree
.
├── CMakeLists.txt
├── doc
│   └── README.md
├── examples
│   ├── aaa.dat
│   ├── bbb.dat
│   ├── ccc.txt
│   ├── ddd.png
│   ├── test1.c
│   ├── test2.c
│   └── test3.c
├── include
│   ├── add.h
│   └── sub.h
├── sh
│   ├── start.sh
│   └── stop.sh
├── src
│   ├── add
│   │   └── add.c
│   ├── CMakeLists.txt
│   └── sub
│       └── sub.c
└── tests
    ├── CMakeLists.txt
    ├── test_add.c
    └── test_sub.c

8 directories, 19 files
->
```

### 2. XZHNetwork/CMakeLists.txt

```cmake
# 1、CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 2、创建一个项目
project (XZHNetwork)

# 3、设置【xxx.h头文件】的include搜索路径
INCLUDE_DIRECTORIES(
  ${PROJECT_SOURCE_DIR}/include
  /usr/local/include
  /usr/include
)

# 4、设置【lib 库文件】的搜索路径
LINK_DIRECTORIES(
  ${PROJECT_BINARY_DIR}/lib
  /usr/local/lib
  /usr/lib
  /lib
)

# 5、添加所有的C源文件的目录
ADD_SUBDIRECTORY(src)
ADD_SUBDIRECTORY(tests)

############### 6、拷贝必要的文件到build目录 ###############

# 6.1 拷贝项目目录下的文档
INSTALL(FILES ${PROJECT_SOURCE_DIR}/doc/README.md DESTINATION doc)

# 6.2 拷贝项目目录下的脚本文件
INSTALL(PROGRAMS ${PROJECT_SOURCE_DIR}/sh/start.sh DESTINATION sh)
INSTALL(PROGRAMS ${PROJECT_SOURCE_DIR}/sh/stop.sh DESTINATION sh)

# 6.3 拷贝项目目录下的库的头文件
INSTALL(FILES ${PROJECT_SOURCE_DIR}/include/add.h DESTINATION include)
INSTALL(FILES ${PROJECT_SOURCE_DIR}/include/sub.h DESTINATION include)

# 6.4 还可以过滤拷examples目录中的txt文件，其他文件全部进行拷贝
INSTALL(DIRECTORY
	${PROJECT_SOURCE_DIR}/examples/ DESTINATION examples
	PATTERN "*.txt" EXCLUDE
	PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ
)

# 6.5 拷贝生成的库文件
# 对应设置放在与生成库的CMakeLists.txt文件中

# 6.6 拷贝生成的可执行文件
# 对应设置放在与生成可执行文件的CMakeLists.txt文件中
```

### 3. XZHNetwork/src/CMakeLists.txt

```cmake
# 项目信息
# project (XZHNetwork)

# 1、设置变量，表示所有的C源文件路径
SET(XZHNetwork_SRCS
  add/add.c
  sub/sub.c
)

################## 2、编译生成动态库 ###################
# 2.1 ADD_LIBRARY(target名字 SHARED 所有的C文件)
ADD_LIBRARY(XZHNetwork SHARED ${XZHNetwork_SRCS})

# 2.2 生成soname版本管理的动态库
SET_TARGET_PROPERTIES(XZHNetwork PROPERTIES VERSION 1.2 SOVERSION 1)

# 2.3 生成动态库后清除掉中间文件
SET_TARGET_PROPERTIES(XZHNetwork PROPERTIES CLEAN_DIRECT_OUTPUT 1)

################## 3、编译生成静态库 ####################
# 3.1 ADD_LIBRARY(target名字 STATIC 所有的C文件)
ADD_LIBRARY(XZHNetwork_static STATIC ${XZHNetwork_SRCS})


# 3.2 设置输出的静态库的文件名
# （否则静态库的名字与target的名字一样为XZHNetwork_static）
SET_TARGET_PROPERTIES(XZHNetwork_static PROPERTIES OUTPUT_NAME "XZHNetwork")

# 3.3 生成静态库后清除掉中间文件
SET_TARGET_PROPERTIES(XZHNetwork_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

############### 4、设置编译生成的【库文件】存放目录 ##################
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

################## 5、拷贝静态库和动态库到安装目录 ####################
INSTALL(TARGETS XZHNetwork LIBRARY DESTINATION lib)
INSTALL(TARGETS XZHNetwork_static ARCHIVE DESTINATION lib/static)
```

### 4. XZHNetwork/tests/CMakeLists.txt

```cmake
################# 1、test1 可执行程序 #################
# 生成可执行程序Test1
ADD_EXECUTABLE(Test1 test_add.c)

# 链接XZHNetwork静态库、libc.so
TARGET_LINK_LIBRARIES(Test1 XZHNetwork;c)

################ 2、test2 可执行程序 ##################
# 生成可执行程序Test2
ADD_EXECUTABLE(Test2 test_sub.c)

# 链接XZHNetwork静态库、libc.so
TARGET_LINK_LIBRARIES(Test2 XZHNetwork;c)

######### 3、设置编译生成的【可执行程序】存放目录 #########
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

############## 4、拷贝可执行文件到安装目录 ###############
INSTALL(TARGETS Test1 RUNTIME DESTINATION bin)
INSTALL(TARGETS Test2 RUNTIME DESTINATION bin)
```

### 5. 新建build目录下  CMAKE_INSTALL_PREFIX 重设安装目录cmake 

```
->  ls
CMakeLists.txt	doc  examples  include	sh  src  tests
->  mkdir build
->  cd build/
->  cmake -D CMAKE_INSTALL_PREFIX=/Users/xiongzenghui/Desktop/xzhnetwork_build ../
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
-- Build files have been written to: /home/xzh/share/XZHNetwork/build
->
```

### 6. build目录下make编译链接

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  make
Scanning dependencies of target XZHNetwork
[ 10%] Building C object src/CMakeFiles/XZHNetwork.dir/add/add.c.o
[ 20%] Building C object src/CMakeFiles/XZHNetwork.dir/sub/sub.c.o
[ 30%] Linking C shared library ../lib/libXZHNetwork.so
[ 30%] Built target XZHNetwork
Scanning dependencies of target XZHNetwork_static
[ 40%] Building C object src/CMakeFiles/XZHNetwork_static.dir/add/add.c.o
[ 50%] Building C object src/CMakeFiles/XZHNetwork_static.dir/sub/sub.c.o
[ 60%] Linking C static library ../lib/libXZHNetwork.a
[ 60%] Built target XZHNetwork_static
Scanning dependencies of target Test1
[ 70%] Building C object tests/CMakeFiles/Test1.dir/test_add.c.o
[ 80%] Linking C executable ../bin/Test1
[ 80%] Built target Test1
Scanning dependencies of target Test2
[ 90%] Building C object tests/CMakeFiles/Test2.dir/test_sub.c.o
[100%] Linking C executable ../bin/Test2
[100%] Built target Test2
->
```

查看当前 build目录 下文件

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  ls bin/
Test1  Test2
->  ls lib/
libXZHNetwork.a  libXZHNetwork.so  libXZHNetwork.so.1  libXZHNetwork.so.1.2
->
```

### 7. build目录下make install拷贝项目文件到安装目录

有可能拷贝到的目录没有权限执行

```
->  pwd
/home/xzh/share/XZHNetwork/build
->  ls
bin  CMakeCache.txt  CMakeFiles  cmake_install.cmake  lib  Makefile  src  tests
->  make install
[ 30%] Built target XZHNetwork
[ 60%] Built target XZHNetwork_static
[ 80%] Built target Test1
[100%] Built target Test2
Install the project...
-- Install configuration: ""
-- Up-to-date: /Users/xiongzenghui/Desktop/xzhnetwork_build/doc/README.md
CMake Error at cmake_install.cmake:36 (file):
  file INSTALL cannot set permissions on
  "/Users/xiongzenghui/Desktop/xzhnetwork_build/doc/README.md"


Makefile:61: recipe for target 'install' failed
make: *** [install] Error 1
```

使用 **sudo** 执行 make install 即可.

```
->  sudo make install
[ 30%] Built target XZHNetwork
[ 60%] Built target XZHNetwork_static
[ 80%] Built target Test1
[100%] Built target Test2
Install the project...
-- Install configuration: ""
-- Up-to-date: /Users/xiongzenghui/Desktop/xzhnetwork_build/doc/README.md
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/sh/start.sh
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/sh/stop.sh
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/include/add.h
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/include/sub.h
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/test3.c
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/ddd.png
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/aaa.dat
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/test2.c
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/bbb.dat
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/examples/test1.c
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/lib/libXZHNetwork.so.1.2
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/lib/libXZHNetwork.so.1
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/lib/libXZHNetwork.so
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/lib/static/libXZHNetwork.a
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/bin/Test1
-- Set runtime path of "/Users/xiongzenghui/Desktop/xzhnetwork_build/bin/Test1" to ""
-- Installing: /Users/xiongzenghui/Desktop/xzhnetwork_build/bin/Test2
-- Set runtime path of "/Users/xiongzenghui/Desktop/xzhnetwork_build/bin/Test2" to ""
->
```

可以看到如上执行了很多的 **Installing** 项。

### 8. 查看安装目录

```
->  cd /Users/xiongzenghui/Desktop/xzhnetwork_build/
->  tree
.
├── bin
│   ├── Test1
│   └── Test2
├── doc
│   └── README.md
├── examples
│   ├── aaa.dat
│   ├── bbb.dat
│   ├── ddd.png
│   ├── test1.c
│   ├── test2.c
│   └── test3.c
├── include
│   ├── add.h
│   └── sub.h
├── lib
│   ├── libXZHNetwork.so -> libXZHNetwork.so.1
│   ├── libXZHNetwork.so.1 -> libXZHNetwork.so.1.2
│   ├── libXZHNetwork.so.1.2
│   └── static
│       └── libXZHNetwork.a
└── sh
    ├── start.sh
    └── stop.sh

7 directories, 17 files
->
```

可以看到在 CMakeLists.txt 文件中指定所有需要进行 INSTALL 安装的文件都拷贝过来了。



## 5. ADD_TEST() 与 ENABLE_TESTING()

### 1. 目录结构

```
->  ls
CMakeLists.txt	main.c
->
->  tree
.
├── CMakeLists.txt
└── main.c

0 directories, 2 files
->
```

### 2. main.c

```c
#include <stdio.h>
#include <assert.h>

int my_atoi(char* str)
{
	int i;
	int num = 0;
	char* p = str;

	for(; *p != 0 ; ++p)
	{
		if (32 == *p)
			break;
		num = num * 10 + (*p - 48);
	}
	return num;
}


int main(int argc, char* argv[])
{
  	// 使用断言来控制测试是否成功
	assert(my_atoi(argv[1]) > 10);
}
```

### 3. CMakeLists.txt 调用 TEST 测试项

```cmake
# 0. CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 1. 项目的名字
PROJECT(HELLO)

# 2. 项目依赖的所有c源文件
SET(SRC_LIST main.c)

# 3. 增加3条对生成的可执行程序a.out的测试，分别执行a.out传递参数值为1、2、100
ADD_TEST(test1 ${PROJECT_BINARY_DIR}/a.out 1)
ADD_TEST(test2 ${PROJECT_BINARY_DIR}/a.out 2)
ADD_TEST(test3 ${PROJECT_BINARY_DIR}/a.out 100)

# 4. enable 启动 test 
ENABLE_TESTING()

# 5. 编译链接所有c文件，生成可执行的二进制程序文件a.out
ADD_EXECUTABLE(a.out ${SRC_LIST})
```

### 4. 新建【build 目录】下执行 cmake、make

```
->  ls
CMakeLists.txt	main.c
->  mkdir build
->  cd build/
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
->  make
Scanning dependencies of target a.out
[ 50%] Building C object CMakeFiles/a.out.dir/main.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
->
```

### 5. 在【build 目录】下执行【make test】来执行在 CMakeLists.txt 添加的【3条测试】

```
->  pwd
/home/xzh/share/makefile/build
->  ls
a.out  CMakeCache.txt  CMakeFiles  cmake_install.cmake	CTestTestfile.cmake  Makefile
->  make test
Running tests...
Test project /home/xzh/share/makefile/build
    Start 1: test1
1/3 Test #1: test1 ............................***Exception: Other  0.17 sec
    Start 2: test2
2/3 Test #2: test2 ............................***Exception: Other  0.18 sec
    Start 3: test3
3/3 Test #3: test3 ............................   Passed    0.00 sec

33% tests passed, 2 tests failed out of 3

Total Test time (real) =   0.36 sec

The following tests FAILED:
	  1 - test1 (OTHER_FAULT)
	  2 - test2 (OTHER_FAULT)
Errors while running CTest
Makefile:61: recipe for target 'test' failed
make: *** [test] Error 8
->
```

- test1、test2 **failed**
- test3 **Passed**
