[TOC]



## 1. 参考文档

http://clang.llvm.org/docs/HowToSetupToolingForLLVM.html

http://clang.llvm.org/docs/LibTooling.html



## 2. 创建一个自己的 tool

### 1. cd $LLVM_SOURCE_ROOT/tools/clang/tools/extra

```
 ~/llvm/tools/clang/tools/extra   release_70  ll
total 32
-rw-r--r--   1 xiongzenghui  staff   775B  6 27 00:51 CMakeLists.txt
-rw-r--r--   1 xiongzenghui  staff   709B  6 27 00:51 CODE_OWNERS.TXT
-rw-r--r--   1 xiongzenghui  staff   2.7K  6 27 00:51 LICENSE.TXT
-rw-r--r--   1 xiongzenghui  staff   1.0K  6 27 00:51 README.txt
drwxr-xr-x   6 xiongzenghui  staff   192B  6 27 00:51 change-namespace
drwxr-xr-x   6 xiongzenghui  staff   192B  6 27 00:51 clang-apply-replacements
drwxr-xr-x  20 xiongzenghui  staff   640B  6 27 00:51 clang-doc
drwxr-xr-x   8 xiongzenghui  staff   256B  6 27 00:51 clang-move
drwxr-xr-x   9 xiongzenghui  staff   288B  6 27 00:51 clang-query
drwxr-xr-x   6 xiongzenghui  staff   192B  6 27 00:51 clang-reorder-fields
drwxr-xr-x  37 xiongzenghui  staff   1.2K  6 27 00:51 clang-tidy
drwxr-xr-x   8 xiongzenghui  staff   256B  6 27 00:51 clang-tidy-vs
drwxr-xr-x  63 xiongzenghui  staff   2.0K  6 27 00:51 clangd
drwxr-xr-x  21 xiongzenghui  staff   672B  6 27 00:51 docs
drwxr-xr-x  19 xiongzenghui  staff   608B  6 27 00:51 include-fixer
drwxr-xr-x  12 xiongzenghui  staff   384B  6 27 00:51 modularize
drwxr-xr-x   6 xiongzenghui  staff   192B  6 27 00:51 pp-trace
drwxr-xr-x  18 xiongzenghui  staff   576B  6 27 00:51 test
drwxr-xr-x   4 xiongzenghui  staff   128B  6 27 00:51 tool-template
drwxr-xr-x  11 xiongzenghui  staff   352B  6 27 00:51 unittests
```

### 2. 创建 tool 工程目录

```
mkdir mytool
cd mytool
touch mytool.cpp
touch CMakeLists.txt
```

```
 ~/llvm/tools/clang/tools/extra/mytool   release_70  tree
.
├── CMakeLists.txt
└── mytool.cpp
```

### 3. mytool/mytool.cpp

```c++

```

### 4. mytool/CMakeLists.txt

```cmake

```

### 5. 修改 $LLVM_SOURCE_ROOT/tools/clang/tools/extra/CMakeLists.txt 添加 mytool 目录编译

```cmake
.........

add_subdirectory(mytool) #=> 末尾追加
```

### 6. 重新生成 LLVM.xcodeproj

```
cd $LLVM_BUILD_ROOT
cmake ../llvm -G Xcode -DCMAKE_BUILD_TYPE=Debug
```

### 7. 打开 LLVM.xcodeproj 选择 mytool target 进行构建

