[TOC]



## 1. 各种 ==编译器/操作系统== 都有 ==自己的== make 实现

- linux gnu make
- qt qmake
- windows ms nmake
- BSD pmake

这些 make 配置文件的编写语法格式 **差异性**, 很难实现 **跨平台**.



## 2. cmake 可以根据 ==不同平台== 生成对应的 ==构建文件==

### 1. CMakeLists.txt

- 按照 **cmake** 语法编写一种与 **平台无关** 的 **CMakeLists.txt** 文件来定制整个 **编译流程**
- 通过编写 **CMakeLists.txt** 中间构建规则文件，来代替直接手写 **各个平台** 对应的构建脚本文件

### 2. CMakeLists.txt => unix/linux Makefile

```
cmake -G "Unix Makefiles" ../llvm
```

### 3. CMakeLists.txt =>Eclipse

```
cmake -G "Eclipse CDT4 - Unix Makefiles" ../llvm
```

### 4. CMakeLists.txt =>Visual Studio

```
CMake -G "Visual Studio 10 Win64" ..\llvm
```

```
CMake -G "Visual Studio 10" ..\llvm
```

### 5. CMakeLists.txt =>Xcode

```
cmake -G "Xcode" ../llvm
```

### 6. CMakeLists.txt =>Ninja

```
cmake -G "Ninja" ../llvm
```

只需要 **编写一次cmake文件** , 即可生成各种 **不同平台** 下的的具体编译规则配置文件。




## 3. cmake Hello World

### 1. 新建一个 CMakeLists.txt 文件，然后编辑

```
touch CMakeLists.txt
vim CMakeLists.txt
```

### 2. 写入如下内容到 CMakeLists.txt 文件中

```cmake
MESSAGE("Hello world")
```

### 3. `cmake -P` 按照 ==脚本模式== 执行 CMakeLists.txt 文件

```
 ~/Desktop/main  cmake -P CMakeLists.txt
Hello world
```

- 参数 `-P` 切换到 **脚本模式** 下运行
- `-P` 后面传入的 **文件名** 没有限制
- `message("字符串")` 在 **控制台** 输出的信息

