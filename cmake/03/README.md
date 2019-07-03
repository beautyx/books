[TOC]



## 1. cmake ==内置== 命令

- CMake 语法中 **没有** **赋值** 语句

- 任何操作 都是通过 **内置命令** 来完成的
	- 1) 赋值
	- 2) 比较
	- 3) 判断

- CMake 中的 内置命令 **不区分** 大小写
	- 但是最好都 **大写**
	- 因为很多内置命令的选项都是 **大写**

- 所有的内置命令的 **函数调用** 形式
	- 1) **括号外面** 的是 **内置命令** 或者叫做 **函数名** 
  - 2) **括号内** 的 **第一个参数** 是调用的 **操作**
  - 3) **后面** 的都是 **函数的参数**，每个参数均以 **空格** 作为间隔

```cmake
内置命令(
  操作1 arg1 arg2 ... argN
  操作2 arg1 arg2 ... argN
  操作3 arg1 arg2 ... argN
)
```



## 2. MESSAGE() 内置指令

### 1. CMakeLists.txt

```cmake
MESSAGE("111111")
MESSAGE(STATUS "222222")
MESSAGE(SEND_ERROR "333333")
MESSAGE(FATAL_ERROR "444444")
```

### 2. cmake 解析 CMakeLists.txt

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
111111
-- 222222
CMake Error at CMakeLists.txt:3 (MESSAGE):
  333333


CMake Error at CMakeLists.txt:4 (MESSAGE):
  444444


-- Configuring incomplete, errors occurred!
See also "/home/xzh/share/makefile/cmake/demo2/CMakeFiles/CMakeOutput.log".
->
```

- 1 与 2 都正常输出，只不过1没有 `—` 的前缀
- 3 报错，但是仍然 **继续** 往下执行
- 4 也报错，但是会 **停止** 往下执行



## 3. cmake 中执行 ==shell 命令==

### 1. cmake 提供执行 ==shell 命令== 调用格式

```cmake
exec_program(Executable [directory in which to run]
             [ARGS <arguments to executable>]
             [OUTPUT_VARIABLE <var>]
             [RETURN_VALUE <var>])
```

### 2. 示例目录结构

```
->  ls
add.c  CMakeLists.txt  main.c  mul.c  sub.c
->  tree
.
├── add.c
├── CMakeLists.txt
├── main.c
├── mul.c
└── sub.c

0 directories, 5 files
->
```

### 3. CMakeLists.txt

```cmake
EXEC_PROGRAM(ls ./ ARGS "-l *.c" OUTPUT_VARIABLE out RETURN_VALUE ret) # ls -l *.c
MESSAGE("out = ${out}")
MESSAGE("ret = ${ret}")
```

#### 3. 脚本模式下执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
out = -rw-rw-r-- 1 xzh xzh   0 1月  19 11:12 add.c
-rw-rw-r-- 1 xzh xzh 274 1月  19 10:57 main.c
-rw-rw-r-- 1 xzh xzh   0 1月  19 11:12 mul.c
-rw-rw-r-- 1 xzh xzh   0 1月  19 11:12 sub.c
ret = 0
->
```
