[TOC]



## 1. 在 ==宏定义== 与 ==函数== 内部, 都通用的 ==内置变量==

| name  | description                                                  |
| ----- | ------------------------------------------------------------ |
| ARGC  | 函数实参的【个数】                                           |
| ARGV  | 所有【实参列表】                                             |
| ARGN  | 所有额外实参列表, 即 ARGV 去掉函数声明时显示指定的实参，剩余的实参 |
| ARGV0 | 函数【第1个】实参                                            |
| ARGV1 | 函数【第2个】实参                                            |
| ARGV2 | 函数【第3个】实参                                            |
| ARGVn | 函数【第n个】实参                                            |



## 2. 宏定义: 内测试如上内置变量

```cmake
# 1. 定义宏函数
macro(sum outvar) # => #define sum(outvar)
	message("ARGC => ${ARGC}")
	message("ARGV => ${ARGV}")
	message("ARGN => ${ARGN}")
	message("ARGV0 => ${ARGV0}")
	message("ARGV1 => ${ARGV1}")
	message("ARGV2 => ${ARGV2}")
	message("ARGV3 => ${ARGV3}")
	message("ARGV4 => ${ARGV4}")
	message("ARGV5 => ${ARGV5}")
	message("ARGV6 => ${ARGV6}")
	SET(${outvar} 100) # 宏函数中向外回传值
endmacro()

# 2. 调用 sum() 传入参数，然后获取返回值写入到 ret 变量中
sum(ret 1 2 3 4 5) #=> ret = sum(1,2,3,4,5)

# 3. 打印宏函数中回传的值
MESSAGE(${ret})
```

```
->  cmake -P CMakeLists.txt
ARGC => 6
ARGV => ret;1;2;3;4;5
ARGN => 1;2;3;4;5
ARGV0 => ret
ARGV1 => 1
ARGV2 => 2
ARGV3 => 3
ARGV4 => 4
ARGV5 => 5
ARGV6 =>
100
->
```



## 3. 宏定义: 求1到10的和

```cmake
macro(sum outvar)
  set(_args ${ARGN})  # 接收除开【第一个参数(返回值变量)】之后的所有的参数
  set(result 0)       # 定义【临时变量】保存【返回值】

  # for 循环 求和
  foreach(_var ${_args})
    math(EXPR result "${result}+${_var}")
  endforeach()

  # return sum 返回总和值
  set(${outvar} ${result})
endmacro()

# 2. ret = sum(1,2,3,4,5)
sum(ret 1 2 3 4 5)
message("Result is :${ret}")
```

cmake 脚本执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
Result is :15
->
```



## 4. 宏定义: ==限制== 参数的 ==个数==

```cmake
MACRO(SUM outvar)
	# 1.
	SET(_args ${ARGN})
	SET(result 0)

	##################################################
	## 2. 限制不能超过4个数字 
	LIST(LENGTH _args len)
	IF(${len} GREATER 4)
		MESSAGE(FATAL_ERROR "to much args!")
	ENDIF()
	##################################################

	# 3. for循环求和
	FOREACH(_var ${_args})
		MATH(EXPR result "${result}+${_var}")
	ENDFOREACH()

	# 4. 回传变量值
	SET(${outvar} ${result})
ENDMACRO()

SUM(ret 1 2 3 4)
MESSAGE('----------- 1 ------------')

SUM(ret 1 2 3 4 5)
MESSAGE('----------- 2 ------------')
```

```
->  cmake -P CMakeLists.txt
'-----------1------------'
CMake Error at CMakeLists.txt:12 (MESSAGE):
  to much args!
Call Stack (most recent call first):
  CMakeLists.txt:27 (SUM)



->
```

第二次调用宏定义传递超过4个参数，直接 **报错结束**.



## 5.  ==函数== 与 ==宏定义== 的区别

### 1. ==函数内== 变量都是 ==局部变量== , 不能像 ==宏定义== 作为 ==返回值==

```cmake
## Foo() 函数
FUNCTION(Foo arg)
	# 函数()内部的都是局部变量
	# 变量arg无法回传到外部
	SET(${arg} "111")
ENDFUNCTION()

Foo(name "ABC") 	# name = Foo('ABC') , 变量name只在函数调用内部能够使用
MESSAGE(${name}) 	# 无法在函数调用外部使用变量name
```

执行 **报错** :

```
->  cmake -P CMakeLists.txt
CMake Error at CMakeLists.txt:9 (message):
  message called with incorrect number of arguments

```

### 2. ==函数内== 无法修改 ==外部变量== 的值

```cmake
# 全局变量 name='ABC'
SET(name 'ABC')

FUNCTION(Foo arg)
	SET(${arg} "111")
ENDFUNCTION()

Foo(name 'ABC') #=> name = Foo('ABC')
MESSAGE(${name})
```

cmake 执行

```
->  cmake -P CMakeLists.txt
ABC
->
```

解除了报错，但是函数内部并 **没有** 修改 **全局变量** 的值.

### 3. 函数接收 ==变量== 的 ==引用==

```cmake
# 全局变量 name='ABC'
SET(name 'ABC')

FUNCTION(Foo arg)
	SET(${arg} "111" PARENT_SCOPE) #=> PARENT_SCOPE 使用外部的作用域，即可【修改外部】的全局变量的值
ENDFUNCTION()

Foo(name 'ABC') #=> name = Foo('ABC')
MESSAGE(${name})
```

cmake 执行输出

```
->  cmake -P CMakeLists.txt
111
->
```
