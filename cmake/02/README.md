[TOC]



## 1. cmake 中的 ==变量== 使用规则

- 变量 **无需声明** 即可使用

- 变量都是一个 **列表(数组)** 类型的变量, 每一个子项都是 **字符串**

- 变量默认都是 **全局** 的

- 变量是 **大小写** 敏感



## 2. 定义 变量

### 1. 语法格式

```cmake
set(变量名 value1 value2 value3 ... valueN)
```

### 2. 示例

```cmake
set(myVar "111" 222 333)
```



## 3.  引用 变量

### 1. 变量 ==值== 的引用

```cmake
${变量名}
```

### 2. 变量 ==本身(地址)== 的引用

```
变量名
```



## 4.  综合示例

### 1. CMakeLists.txt

```cmake
# 1. 定义变量
set(myVar "111" 222 333)

# 2. 直接当做字符串输出
message(myVar)

# 3. 引用变量的值
message(${myVar})
message("${myVar}")
```

### 2. 执行 cmake

```
->  cmake -P CMakeLists.txt
myVar
111222333
111;222;333
->
```



## 5. ==环境(ENV)== 变量

### 1. 读取

```cmake
MESSAGE(STATUS "HOME dir: $ENV{HOME}")
```

### 2. 写入

```cmake
SET(ENV{变量名} 值)
```



## 6. ==内置== 变量

```make
1, CMAKE_MAJOR_VERSION, CMAKE 主版本号,比如 2.4.6 中的 2 
2, CMAKE_MINOR_VERSION, CMAKE 次版本号,比如 2.4.6 中的 4 
3, CMAKE_PATCH_VERSION, CMAKE补丁等级,比如2.4.6 中的6 
4, CMAKE_SYSTEM, 系统名称,比如 Linux-2.6.22 
5, CMAKE_SYSTEM_NAME, 不包含版本的系统名,比如 Linux 
6, CMAKE_SYSTEM_VERSION, 系统版本,比如 2.6.22 
7, CMAKE_SYSTEM_PROCESSOR, 处理器名称,比如 i686. 
8, UNIX, 在所有的类UNIX平台为TRUE,包括OS X和cygwin 
9, WIN32, 在所有的 win32 平台为 TRUE,包括 cygwin
```
