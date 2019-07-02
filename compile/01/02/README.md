[TOC]



##  List

![](0.png)



## 1. 被隐藏了的 4个 过程

![](01.png)



## 2. GCC 编译过程分解 (实际上不止4个步骤)

![](02.png)



## 3. GCC 是一系列内部命令的包装

![](05.png)



## 4. 4大过程

### 1. 预编译

![](03.png)

### 2. 编译

![](04.png)

### 3. 汇编

![](06.png)

### 4. 链接

![](07.png)

- 1) 直接使用 **ld** 链接生成 **可执行文件** 命令相当的长
- 2) 所以才会有 **gcc *.o** 这样的 **包装** 



## 5. 编译器

### 0. 编译过程 6大步

![](08.png)

### 1. 词法分析

![](09.png)

![](10.png)

### 2. 语法分析

![](11.png)

![](12.png)

### 3. 语义分析

![](13.png)

![](14.png)

![](15.png)

### 4. 语法树 (源码级) ==优化== (opt)

![](16.png)

### 5. 生成 ==中间== 代码

![](17.png)

### 6. 编译器 ==前端== <--> 中间代码 <--> 编译器 ==后端==

![](18.png)

与 LLVM 编译器模型一致。

### 7. 生成 ==目标机器代码== (Code Generator)

![](19.png)

### 8. 目标机器代码 ==优化==

![](20.png)

### 9. 总结: ==编译器==

![](21.png)

![](22.png)



## 6. ==链接器== 将所有的 xx.o 目标文件 链接在一起

![](23.png)

## 7. 模块拼接 : ==静态== 拼接

![](24.png)

![](25.png)

## 8. 地址重定位

![](26.png)

![](27.png)


