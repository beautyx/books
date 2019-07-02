[TOC]



## 1. 修改 LLVM 库中的 PassManagerBuilder.cpp

### 1. PassManagerBuilder.cpp 文件全路径

```
$LLVM_SOURCE_ROOT/lib/Transforms/IPO/PassManagerBuilder.cpp
```

### 2. 添加 Pass 头文件

```
#include "llvm/Transforms/Obfuscation/SimplePass.h"
```

### 3. 添加 实例化 Pass 的代码

可按照已有的代码进行修改:

```c
static cl::opt<bool> ExtraVectorizerPasses(
    "extra-vectorizer-passes", cl::init(false), cl::Hidden,
    cl::desc("Run cleanup optimization passes after vectorization."));
```

比如:

```c
static cl::opt<bool> SimplePass("simplepass", cl::init(false),
                           cl::desc("Enable simple pass"));
```

### 4. PassManagerBuilder::populateModulePassManager() 方法中，注册 实例化 Pass 的代码

```c
MPM.add(createSimplePass(SimplePass));
```

末尾添加即可.



## 2. 注册 Pass 的编译路径

### 1. 进入 所有 pass 目录

```
cd $LLVM_SOURCE_ROOT/lib/Transforms/
```

```
ll
total 16
drwxr-xr-x   7 xiongzenghui  staff   224B  6 27 00:48 AggressiveInstCombine
-rw-r--r--   1 xiongzenghui  staff   361B  6 28 16:37 CMakeLists.txt
drwxr-xr-x  12 xiongzenghui  staff   384B  6 27 00:48 Coroutines
drwxr-xr-x   5 xiongzenghui  staff   160B  6 27 00:48 Hello
drwxr-xr-x  39 xiongzenghui  staff   1.2K  6 27 00:48 IPO
drwxr-xr-x  19 xiongzenghui  staff   608B  6 27 00:48 InstCombine
drwxr-xr-x  21 xiongzenghui  staff   672B  6 27 00:48 Instrumentation
-rw-r--r--   1 xiongzenghui  staff   805B  6 27 23:56 LLVMBuild.txt
drwxr-xr-x   5 xiongzenghui  staff   160B  6 28 17:05 Obfuscation
drwxr-xr-x  19 xiongzenghui  staff   608B  6 27 00:48 ObjCARC
drwxr-xr-x  73 xiongzenghui  staff   2.3K  6 27 00:48 Scalar
drwxr-xr-x  62 xiongzenghui  staff   1.9K  6 27 00:48 Utils
drwxr-xr-x  22 xiongzenghui  staff   704B  6 27 00:48 Vectorize
```

其中的 **Obfuscation 目录** 就是要注册到 **PassManager** 中的 Pass

### 2. Obfuscation (LLVM Pass)

```shell
# ~/llvm/lib/Transforms/Obfuscation   release_70 ●  tree
.
├── CMakeLists.txt
├── LLVMBuild.txt
└── SimplePass.cpp
```

### 3. Obfuscation/CMakeLists.txt

```cmake
add_llvm_loadable_module( LLVMObfuscation
  SimplePass.cpp

  DEPENDS
  intrinsics_gen
  PLUGIN_TOOL
  opt
)
```

- 1) 库的名字 LLVMObfuscation => LLVMObfuscation.dylib
- 2) 依赖 SimplePass.cpp 源文件

### 4. 将 LLVMObfuscation 注册到 $LLVM_SOURCE_ROOT/lib/Transforms/IPO/LLVMBuild.txt

#### 1. $LLVM_SOURCE_ROOT/lib/Transforms/IPO/LLVMBuild.txt 原来的内容

```makefile
[component_0]
type = Library
name = IPO
parent = Transforms
library_name = ipo
required_libraries = AggressiveInstCombine Analysis BitReader BitWriter Core InstCombine IRReader Linker Object ProfileData Scalar Support TransformUtils Vectorize Instrumentation
```

#### 2. 在 required_libraries 末尾追加 LLVMObfuscation


```makefile
[component_0]
..................

required_libraries = ............ LLVMObfuscation
```



## 3. 加载 pass

```
$LLVM_BUILD_ROOT/Debug/bin/clang \
	-mllvm \
	-simplepass \
	test.c \
	-o after_test
```



