[TOC]



## 1. if/else

### 1. 条件表达式判断

#### 1. if 条件

```cmake
if(表达式)
	....
endif()
```

#### 2. if `! 条件`

```cmake
if(NOT 表达式)	# not 必须【大写】
	....
endif()
```

#### 3. if 条件1 or 条件2

```cmake
if(表达式1 OR 表达式2) # OR 必须【大写】
	....
endif()
```

#### 4. if 条件1 and 条件2

```cmake
if(表达式1 AND 表达式2) # AND 必须【大写】
	....
endif()
```

#### 5. if command(op arg1 arg2 …)

```cmake
if(内置命令名(函数名 参数1 参数2...参数n))
	....
endif()
```

### 2. 【数值】比较

```cmake
IF (variable LESS number)
IF (string LESS number)
IF (variable GREATER number)
IF (string GREATER number)
IF (variable EQUAL number)
IF (string EQUAL number)
```

### 3. 【字母表顺序】比较

```cmake
IF (variable STRLESS string)
IF (string STRLESS string)
IF (variable STRGREATER string)
IF (string STRGREATER string)
IF (variable STREQUAL string)
IF (string STREQUAL string)
```

### 4. 【文件类型】判断 

#### 1. EXISTS

```cmake
IF (EXISTS dir)
	....
ENDIF()

IF (EXISTS file) 
	....
ENDIF()
```

#### 2. is dir

```cmake
# 当dir是目录时,为真
IF (IS_DIRECTORY dir) 
	....
ENDIF()
```

#### 3. is newer than

```cmake
# 当file1比file2新
# 文件名需使用全路径
IF (file1 IS_NEWER_THAN file2)
	....
ENDIF()
```

### 5. 对于 cmake【内置变量】判断

```cmake
if(WIN32)
  message("platform is windows")
elseif(APPLE)
  message("platform is APPLE")
elseif(LINUX)
  message("platform is LINUX")
elseif(UNIX)
  message("platform is UNIX")
endif()
```

cmake 运行输出

```
->  cmake -P CMakeLists.txt
platform is APPLE
->
```



## 2. 循环

### 1. CMake 循环有两种

#### 1. foreach

```cmake
foreach()
  ...
endforeach()
```

#### 2. While

```cmake
while()
  ...
endwhile()
```

### 2. 示例1：遍历 list 数组

#### 1. CMakeLists.txt

```cmake
SET(mylist "a" "b" c "d")

FOREACH(_var ${mylist})
  MESSAGE("item = ${_var}")
ENDFOREACH()
```

#### 2. `cmake -p` 脚本模式执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
item = a
item = b
item = c
item = d
->
```

### 3. 示例2：求1加到10的和

#### 1. CMakeLists.txt

```cmake
# 1. 全局变量 result = 0
SET(result 0)

# 2. 求1到10的和
FOREACH(_var RANGE 1 10)
  MATH(EXPR result "${result}+${_var}") #=> result = result + _var
ENDFOREACH()

# 3. 打印和
MESSAGE("1 +...+ 10  = ${result}")
```

#### 2. `cmake -p` 脚本模式执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
1 +...+ 10  = 55
->
```

### 4. 示例3：获取 list 的长度

#### 1. CMakeLists.txt

```cmake
SET(mylist a b c d )
LIST(LENGTH mylist _length) # => _length = length(mylist)
MESSAGE("列表中元素的个数为:${_length}")
```

#### 2. `cmake -p` 脚本模式执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
列表中元素的个数为:4
->
```

### 5. 示例4：list 逆序

#### 1. CMakeLists.txt

```makefile
SET(strList "a" "b" "c" "d")
SET(reverList ${strList})
LIST(REVERSE reverList)	# list(函数名 参数) 其中的函数名必须[大写]
MESSAGE("${reverList}")
```

#### 2. `cmake -p` 脚本模式执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
d;c;b;a
->
```

### 6. 示例5：list 追加

#### 1. CMakeLists.txt

```cmake
SET(list1 "a" "b" "c" "d")
SET(list2 "1" "2" "3" "4")

# 1. list1追加list2
FOREACH(_var ${list2})
    LIST(APPEND list1 ${_var})
ENDFOREACH()

# 2. 
MESSAGE(STATUS "${list1}")
```

#### 2. `cmake -p` 脚本模式执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
-- a;b;c;d;1;2;3;4
->
```

