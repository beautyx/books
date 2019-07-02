[TOC]



## 01. C/C++

### 01. 编写高质量代码++改善C程序代码的125个建议

- [01. 初识](./125_Suggestions_Writing_High_Quality_Code_for_c/01/initial.md)

- [02. 数据类型](./125_Suggestions_Writing_High_Quality_Code_for_c/02/data_type.md)

- [03. 表达式](./125_Suggestions_Writing_High_Quality_Code_for_c/03/expression.md)

- [04. 函数](./125_Suggestions_Writing_High_Quality_Code_for_c/04/function.md)

- [05. 指针](./125_Suggestions_Writing_High_Quality_Code_for_c/05/pointer.md)

- [06. 数组 与 指针](./125_Suggestions_Writing_High_Quality_Code_for_c/06/array_pointer.md)

- [07. 结构体、位域、枚举](./125_Suggestions_Writing_High_Quality_Code_for_c/07/struct.md)

- [08. 字符串](./125_Suggestions_Writing_High_Quality_Code_for_c/08/string.md)

- [09. 文件系统](./125_Suggestions_Writing_High_Quality_Code_for_c/09/io.md)

- [10. 预处理](./125_Suggestions_Writing_High_Quality_Code_for_c/10/pre_processing.md)

- [11. 断言与异常](./125_Suggestions_Writing_High_Quality_Code_for_c/11/assert.md)

### 02. C指针 基本概念 核心技术及最佳实践

- [01. 内存](./c_pointer_best_practices/01/memory.md)

- [02. 指针基础](./c_pointer_best_practices/02/point_basic.md)

- [03. 函数指针](./c_pointer_best_practices/03/point_function.md)

### 03. Effective C++: 改善程序与设计的55个具体做法(第3版)

- [README](./effective_c++/effective.md)

### 04. More Effective C++: 35个改善编程与设计的有效方法

- [README](./more_effective_c++/effective.md)

### 05. EFFECTIVE+STL: 50条有效使用STL的经验

- [README](./effective_stl/effective.md)




## 02. compiler

### 01. 程序员的自我修养

- [01 - 温故而知新](./compile/01/01/README.md)

- [02 - 编译、链接](./compile/01/02/README.md)

- [03 - 目标文件](./compile/01/03/README.md)

- [04 - ==静态== 链接](./compile/01/04/README.md)

- [05 - 可执行文件的 ==装载== 与 ==进程==](./compile/01/05/README.md)

- [06 - ==动态== 链接](./compile/01/06/README.md)

- [07 - Linux ==共享库==](./compile/01/07/README.md)

- [08 - 内存](./compile/01/08/README.md)

- [09 - 运行库](./compile/01/09/README.md)

- [10 - 系统调用](./compile/01/10/README.md)

- [11 - 运行库实现](./compile/01/11/README.md)

- [12 - 结束篇](./compile/01/12/README.md)

### 02. 高级C/C++编译技术

- [00 - 序言](./compile/02/00/README.md)

- [01 - 多任务操作系统基础](./compile/02/01/README.md)

- [02 - 程序的声明周期](./compile/02/02/README.md)

- [04 - 加载程序执行](./compile/02/04/README.md)

- [05 - 重用(库)](./compile/02/05/README.md)

### 03. 使用 linux 汇编语言



### 04. LLVM ==Cookbook==

- [01 - LLVM 设计 与 使用](./compile/03/01/README.md)

- [02 - 实现 编译器 前端(clang)](./compile/03/02/README.md)

- [03 - 扩展 编译器 前端 & 增加 JIT 支持](./compile/03/03/README.md)

- [04 - IR 优化器](./compile/03/04/README.md)

- [05 - 与平台无关的 代码生成器](./compile/03/05/README.md)

### 05. LLVM 个人学习笔记

#### 01. LLVM ==架构==

- [README](./compile/04/05/README.md)

#### 02. clang 编译器 ==前端==

##### 01. clang 基础使用

- [README](./compile/04/03/README.md)

##### 02. ==plugin==

- 针对 **语法树(AST)** 有 **完全** 的 **控制权**

- 可作为 **插件** 注入到 **编译流程** 中

- 可以 **影响** build 过程

- 针对 **语法树(AST)** 定义一些: 编码规范检查, 代码风格检查，命名检查 ...

- [README](./compile/04/02/README.md)

##### 03. Static Analyzer ==Checker==

- [README](./compile/04/06/README.md)

##### 04. LibTooling

- 针对 **语法树(AST)** 有 **完全** 的 **控制权**

- 可以作为一个 **单独的命令** 使用 (clang-format main.m)

- 也可以自己实现一个类似 LibTooling 这样的工具, 来实现 **遍历、访问、修改** 语法树 中的任意节点

- 甚至做一个 **代码的转换器** , 比如实现 objc 转换成 swift、js、...

- [README](./compile/04/07/README.md)

#### 03. LLVM IR (Intermediate Representation) 中间指令

##### 01. IR 语法

- [README](./compile/04/04/README.md)

#### 04. 编写 ==Pass== 优化(opt) IR

##### 01. Pass

- [README](./compile/04/01/README.md)

##### 02. Pass==Manager==

- [README](./compile/04/09/README.md)

#### 05. LLVM ==函数库==

##### 01. libclang

- [README](./compile/04/08/README.md)

#### 99. WWDC 2019 whats_new_in_clang_and_llvm

- [README](./compile/04/99/README.md)



## 03. iOS

### 01. 启动优化



### 02. 组件化

- [0 - 组件的层次](./ios/02/01/README.md)

- [0 - 组件 (业务) 工程结构](./ios/02/01/README.md)

- [0 - 组件 (业务) 通信](./ios/02/01/README.md)

- [0 - 组件 开发、调试、发布、集成、版本管理](./ios/02/01/README.md)

- [0 - 组件 MR 构建、各项数据分析、质量控制](./ios/02/01/README.md)

- [0 - 组件 日志](./ios/02/01/README.md)

- [0 - 提高 工程构建 的自动化程度、便利性](./ios/02/01/README.md)

- [0 - 百度 easybox](./ios/02/01/README.md)

- [0 - 提升 组件化 日常开发效率](./ios/02/01/README.md)

### 03. 包体积分析与优化

- [README](./ios/04/README.md)

### 04. cocoapods 源码阅读

- [01 - Gemfile 与 cocoapods.gemspec](./ruby/cocoapods/01/README.md)

- [02 - pod install](./ruby/cocoapods/02/README.md)

- [03 - pod](./ruby/cocoapods/03/README.md)



## 04. devops

### 01. Ansible

最近因为做包体积自动化分析工具，就涉及到 CI/CD 打包环境进行修改，于是顺利的搞出了问题 … 解决的过程中，就一直想有没有什么工具能够减轻恢复环境的痛苦 ..

于是找到了 ansible !

[01 - ansible start](./ansible/01/ansible_start.md)

[02 - inventory](./ansible/02/ansible_inventory.md)

[03 - modules - 01 - 学会使用 ansible 模块](./ansible/03/01/ansible_modules.md)

[03 - modules - 02 - 常用模块](./ansible/03/02/ansible_modules.md)

[04 - playbook - 01 - playbook 是什么？](./ansible/04/01/ansible_playbook.md)

[04 - playbook - 02 - tasks](./ansible/04/02/ansible_playbook.md)

[04 - playbook - 03 - handlers](./ansible/04/03/ansible_playbook.md)

[04 - playbook - 04 - tags](./ansible/04/04/ansible_playbook.md)

[04 - playbook - 05 - 变量](./ansible/04/05/ansible_playbook.md)

[05 - playbook - 01 - 循环](./ansible/05/01/ansible_playbook.md)

[05 - playbook - 02 - 判断 - when](./ansible/05/02/ansible_playbook.md)

[05 - playbook - 03 - 判断 - test](./ansible/05/03/ansible_playbook.md)

[06 - playbook - 01 - 过滤器](./ansible/06/ansible_playbook.md)

[07 - playbook - 01 - 角色](./ansible/07/ansible_playbook.md)


