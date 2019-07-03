[TOC]




## 1. ==内部== 编译

直接在【CMakeLists.txt 文件 **所在目录**】下执行 cmake 解析 **生成 Makefile** 等构建规则文件:

```
->  ls
CMakeLists.txt	main.c	tool.c
->
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

```
->  ls -l
total 40
-rw-rw-r-- 1 xzh xzh 11560 1月  18 22:58 CMakeCache.txt
drwxrwxr-x 5 xzh xzh  4096 1月  18 22:58 CMakeFiles
-rw-rw-r-- 1 xzh xzh  1383 1月  18 22:58 cmake_install.cmake
-rw-rw-r-- 1 xzh xzh   628 1月  18 20:56 CMakeLists.txt
-rw-rw-r-- 1 xzh xzh    74 1月  18 20:47 main.c
-rw-rw-r-- 1 xzh xzh  5313 1月  18 22:58 Makefile
-rw-rw-r-- 1 xzh xzh    60 1月  18 20:46 tool.c
->
```

- 最终直接在 **当前目录下** 生成了很多的 cmake **临时文件**

- 如果继续执行 **make** 还可能生成更多的 **编译过程** 中产生的 **中间文件**

- 所以 **内部编译** 会 **污染** 当前目录



## 2. ==外部== 编译: 使用 ==1级== 子目录

### 1. 创建用于【cmake 生成中间文件】的 build 目录

```
->  ls
CMakeLists.txt	main.c	tool.c
->
->  mkdir build
```

### 2. 目录结构如下

```
->  tree
.
├── build
├── CMakeLists.txt
├── main.c
└── tool.c

1 directory, 3 files
->
```

### 3. 进入到【build 目录】执行 cmake 解析 ==上层目录== 中的 CMakeLists.txt

```
->  ls
build  CMakeLists.txt  main.c  tool.c
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
-- ==> HELLO Binary dir = /home/xzh/share/makefile/cmake/demo1/build
-- ==> HELLO Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1/build
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/cmake/demo1/build
->
```

- 最后一行提示 **Build files have been written to: /home/xzh/share/makefile/cmake/demo1/build**
- 所有的中间文件都写入到了 **demo1/build/** 目录下，而不是 **demo1/** 主目录下

### 4. 在【build 目录】下继续执行 make 编译生成库、脚本、可执行程序

```
->  pwd
/home/xzh/share/makefile/cmake/demo1/build
->  ls
CMakeCache.txt	CMakeFiles  cmake_install.cmake  Makefile
->  make
Scanning dependencies of target a.out
[ 33%] Building C object CMakeFiles/a.out.dir/main.c.o
[ 66%] Building C object CMakeFiles/a.out.dir/tool.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
->  ./a.out
--- run ---
->
```

这样生成的所有中间文件，都会在 **build**子目录中，不会在顶层目录。

### 5. 查看主目录下

```
->  pwd
/home/xzh/share/makefile/cmake/demo1/build
->  cd ..
->  pwd
/home/xzh/share/makefile/cmake/demo1
->  ls
CMakeLists.txt	main.c	subdir	tool.c
```

主目录下很 **干净**。



## 3. ==外部== 编译: 使用 ==2级== 子目录

### 1. 创建用于 cmake 生成中间文件的 subdir/build/ 2级目录

```shell
->  ls
CMakeLists.txt	main.c	tool.c
->
->  mkdir subdir
->  cd subdir/
->  mkdir build
->
```

### 2. 目录结构

```
->  ls
CMakeLists.txt	main.c	subdir	tool.c
->  tree
.
├── CMakeLists.txt
├── main.c
├── subdir
│   └── build
└── tool.c

2 directories, 3 files
->
```

### 3. 进入到【 subdir/build/ 目录】下执行 cmake

```cmake
->  ls
CMakeLists.txt	main.c	subdir	tool.c
->  cd subdir/
->  cd build/
```

```
->  cmake ../../
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
-- ==> HELLO Binary dir = /home/xzh/share/makefile/cmake/demo1/subdir/build
-- ==> HELLO Source dir = /home/xzh/share/makefile/cmake/demo1
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1/subdir/build
-- ==> PROJECT Source dir = /home/xzh/share/makefile/cmake/demo1
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/cmake/demo1/subdir/build
->
```

最后提示所有文件写入到 `/home/xzh/share/makefile/cmake/demo1/subdir/build`

### 4. 【subdir/build/ 目录】下继续执行 make 

```
->  pwd
/home/xzh/share/makefile/cmake/demo1/subdir/build
->  ls
CMakeCache.txt	CMakeFiles  cmake_install.cmake  Makefile
->  make
Scanning dependencies of target a.out
[ 33%] Building C object CMakeFiles/a.out.dir/main.c.o
[ 66%] Building C object CMakeFiles/a.out.dir/tool.c.o
[100%] Linking C executable a.out
[100%] Built target a.out
->  ./a.out
--- run ---
->
```

### 5. 查看主目录下

```
->  pwd
/home/xzh/share/makefile/cmake/demo1/subdir/build
->  cd ..
->  cd ..
->  pwd
/home/xzh/share/makefile/cmake/demo1
->  ls
CMakeLists.txt	main.c	subdir	tool.c
```

主目录下很 **干净**.

