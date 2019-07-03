[TOC]



## 1. FILE 函数

### 1. 目录结构

```
->  ls
CMakeLists.txt
->
```

### 2. CMakeLists.txt

```makefile
# 1. 打开或创建 main.c
FILE(WRITE main.c "#include <stdio.h>\n")

# 2. 追加写入文件 main.c
FILE(APPEND main.c "int main()\n")
FILE(APPEND main.c "{\n")
FILE(APPEND main.c " printf(\"Hello\\n\");\n")
FILE(APPEND main.c "}\n")

# 3. 读取文件 main.c
FILE(READ main.c txt)
MESSAGE(${txt})

# 4. 执行gcc main.c
EXEC_PROGRAM(gcc ./ ARGS "main.c" OUTPUT_VARIABLE out RETURN_VALUE ret)
MESSAGE("out = ${out}")
MESSAGE("ret = ${ret}")
MESSAGE("------------------")

# 5. 执行 ./a.out
EXEC_PROGRAM(./a.out OUTPUT_VARIABLE out RETURN_VALUE ret)
MESSAGE("out = ${out}")
MESSAGE("ret = ${ret}")

# 6. 删除 a.out
FILE(REMOVE a.out)
```

### 2. 脚本模式 执行 CMakeLists.txt

```
->  cmake -P CMakeLists.txt
#include <stdio.h>
int main()
{
 printf("Hello\n")
}

out =
ret = 0
------------------
out = Hello
ret = 0
->
```

### 3. 查看目录下生成的文件

```
->  ls
CMakeLists.txt	main.c
->
```

a.out 被删除了。

更多的FILE用法查看cmake官方文档。



## 2. FIND 函数

### 1. cmake FIND CURL

#### 1. 目录结构

```
->  ls
CMakeLists.txt	main.c
->  tree
.
├── CMakeLists.txt
└── main.c

0 directories, 2 files
->
```

#### 2. main.c 中依赖 ==libCurl 库==

```c
#include <curl/curl.h> #依赖 libCurl 库
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

FILE *fp;

int write_data(void *ptr, size_t size, size_t nmemb, void *stream)
{
	int written = fwrite(ptr, size, nmemb, (FILE *)fp);
	return written;
}

int main()
{
	const char * path = “/tmp/curl-test”;
	const char * mode = “w”;
	fp = fopen(path,mode);
	curl_global_init(CURL_GLOBAL_ALL);
	CURLcode res;
	CURL *curl = curl_easy_init();
	curl_easy_setopt(curl, CURLOPT_URL, “http://www.linux-ren.org”);
	curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_data);
	curl_easy_setopt(curl, CURLOPT_VERBOSE, 1);
	res = curl_easy_perform(curl);
	curl_easy_cleanup(curl);
}
```

#### 3. CMakeLists.txt 查找是否存在【libCurl 库】来决定【是否链接】libCurl 库

```makefile
# 1.
cmake_minimum_required (VERSION 2.8)

# 2.
PROJECT(CURLTEST)

# 3. 查找是否存在 curl 库
FIND_PACKAGE(CURL)

# 4. 再决定是否 链接
IF(CURL_FOUND)
	INCLUDE_DIRECTORIES(${CURL_INCLUDE_DIR}) # 包含curl库头文件路径
	ADD_EXECUTABLE(curtest main.c) # 设置编译链接mian.c生成的可执行程序target
	TARGET_LINK_LIBRARIES(curtest ${CURL_LIBRARY}) # 给target链接curl库
ELSE(CURL_FOUND)
	MESSAGE(FATAL_ERROR "CURL library not found")
ENDIF(CURL_FOUND)
```

#### 4. 新建 build 目录，进入执行 cmake

```
->  ls
CMakeLists.txt	main.c
->  mkdir build
->  cd build/
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
-- Could NOT find CURL (missing:  CURL_LIBRARY CURL_INCLUDE_DIR)
CMake Error at CMakeLists.txt:16 (MESSAGE):
  CURL library not found


-- Configuring incomplete, errors occurred!
See also "/home/xzh/share/makefile/build/CMakeFiles/CMakeOutput.log".
->
```

报错提示 **CURL library not found**

### 2. 自定义类似 FIND CURL 

#### 1. Find  CURL headers and libraries 提供4个变量

```
CURL_INCLUDE_DIRS   - where to find curl/curl.h, etc.
CURL_LIBRARIES      - List of libraries when using curl.
CURL_FOUND          - True if curl found.
CURL_VERSION_STRING - the version of curl found (since CMake 2.8.8)
```

#### 2. 模仿 find curl 实现

#### 1. 目录结构

```
->  ls
CMakeLists.txt	main.c	mysdk
->  tree
.
├── CMakeLists.txt
├── main.c
└── mysdk
    ├── cmake
    │   └── FindMYSDK.cmake
    ├── include
    │   ├── add.h
    │   └── sub.h
    └── lib
        └── libMySdk.a

4 directories, 6 files
->
```

#### 2. main.c

```c
#include <stdio.h>
#include <add.h>
#include <sub.h>
int main()
{
	printf("%d\n", add(4,5));
	printf("%d\n", sub(4,5));
}
```

#### 3. CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 2.8)

# 1. 让当前 CMakeLists.txt 文件能够找到 FindMYSDK.cmake 文件
SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/mysdk/cmake)
MESSAGE(STATUS "==> CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}")

# 2 调用 FindMYSDK.cmake 文件
FIND_PACKAGE(MYSDK)
MESSAGE(STATUS "==> MYSDK_FOUND = ${MYSDK_FOUND}")

# 3. 只有当找到 MYSDK 库头文件和库文件，才对 mian.c 编译链接
IF(MYSDK_FOUND)
	INCLUDE_DIRECTORIES(${MYSDK_INCLUDE_DIR})
	ADD_EXECUTABLE(mysdk_app main.c)
	TARGET_LINK_LIBRARIES(mysdk_app ${MYSDK_LIBRARY})
ENDIF(MYSDK_FOUND)
```

#### 4. mysdk/cmake/FindMYSDK.cmake

```makefile
# 1. 指定头文件搜索路径 => MYSDK_INCLUDE_DIR
# - 1) 这里只是为了测试，所以判断头文件路径为自定义目录
# - 2) 一般是 /usr/include、/usr/local/include
FIND_PATH(MYSDK_INCLUDE_DIR NAMES add.h sub.h PATHS /home/xzh/share/makefile/mysdk/include NO_DEFAULT_PATH)
MESSAGE(STATUS "==> MYSDK_INCLUDE_DIR = ${MYSDK_INCLUDE_DIR}")

# 2. 指定库文件搜索路径 => MYSDK_LIBRARY
# - 1) 同样是为了测试，所以判断头文件路径为自定义目录，一般是 /lib、/usr/lib、/usr/local/lib
# - 2) NAMES后面跟的【库名】格式：libMySdk.a ==> MySdk
FIND_LIBRARY(MYSDK_LIBRARY NAMES MySdk PATHS /home/xzh/share/makefile/mysdk/lib)
MESSAGE(STATUS "==> MYSDK_LIBRARY = ${MYSDK_LIBRARY}")

# 3. 查找 mysdk 的头文件和库文件 => MYSDK_FOUND
IF (MYSDK_INCLUDE_DIR AND MYSDK_LIBRARY)
	SET(MYSDK_FOUND TRUE)
ENDIF (MYSDK_INCLUDE_DIR AND MYSDK_LIBRARY)

# 4. 根据上面得到的MYSDK_FOUND变量值，决定是否继续执行cmake流程
IF (MYSDK_FOUND)
	IF (NOT MYSDK_FIND_QUIETLY) # 规定如果不指定quiet参数，则输出如下信息
		MESSAGE(STATUS "==> Found mysdk: ${MYSDK_LIBRARY}")
	ENDIF (NOT MYSDK_FIND_QUIETLY)
ELSE (MYSDK_FOUND)
	IF (MYSDK_FIND_REQUIRED) # 库文件如果是必须的，则让cmake报错结束执行
		MESSAGE(FATAL_ERROR "==> Could not find mysdk library") # cmake报错结束执行
	ENDIF (MYSDK_FIND_REQUIRED)
ENDIF (MYSDK_FOUND)
```

#### 5. 新建build目录执行cmake

```
->  ls
CMakeLists.txt	main.c	mysdk
->  mkdir build
->  cd build/
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
-- ==> CMAKE_MODULE_PATH = /home/xzh/share/makefile/mysdk/cmake
-- ==> MYSDK_INCLUDE_DIR = /home/xzh/share/makefile/mysdk/include
-- ==> MYSDK_LIBRARY = /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> Found mysdk: /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> MYSDK_FOUND = TRUE
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/build
->
```

#### 6. 打印的变量输出

```
-- ==> CMAKE_MODULE_PATH = /home/xzh/share/makefile/mysdk/cmake
-- ==> MYSDK_INCLUDE_DIR = /home/xzh/share/makefile/mysdk/include
-- ==> MYSDK_LIBRARY = /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> Found mysdk: /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> MYSDK_FOUND = TRUE
```

- **include** 和 **lib** 路径都已搜索到
- **Found mysdk:**

#### 7. 继续在 build目录 make

```
->  pwd
/home/xzh/share/makefile/build
->  make
Scanning dependencies of target mysdk_app
[ 50%] Building C object CMakeFiles/mysdk_app.dir/main.c.o
[100%] Linking C executable mysdk_app
[100%] Built target mysdk_app
->  ./mysdk_app
9
-1
->
```

最后编译链接生成可执行程序，且正确执行

#### 8. FIND_PACKAGE(MYSDK QUIET) 

#### 修改 CMakeLists.txt 为如下，在 FIND_PACKAGE() 增加一个参数 QUIET

```cmake
cmake_minimum_required(VERSION 2.8)

# 1. 让当前CMakeLists.txt文件能够找到FindMYSDK.cmake文件
SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/mysdk/cmake)
MESSAGE(STATUS "==> CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}")

# 2 调用FindMYSDK.cmake文件
FIND_PACKAGE(MYSDK QUIET)
MESSAGE(STATUS "==> MYSDK_FOUND = ${MYSDK_FOUND}")

# 3. 只有当找到MYSDK库头文件和库文件，才对mian.c编译链接
IF(MYSDK_FOUND)
	INCLUDE_DIRECTORIES(${MYSDK_INCLUDE_DIR})
	ADD_EXECUTABLE(mysdk_app main.c)
	TARGET_LINK_LIBRARIES(mysdk_app ${MYSDK_LIBRARY})
ENDIF(MYSDK_FOUND)
```

#### 重新在 build目录 下 cmake、make

```
->  ls
CMakeLists.txt	main.c	mysdk
->  mkdir build
->  cd build/
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
-- ==> CMAKE_MODULE_PATH = /home/xzh/share/makefile/mysdk/cmake
-- ==> MYSDK_INCLUDE_DIR = /home/xzh/share/makefile/mysdk/include
-- ==> MYSDK_LIBRARY = /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> MYSDK_FOUND = TRUE
-- Configuring done
-- Generating done
-- Build files have been written to: /home/xzh/share/makefile/build
->
```

#### 打印的变量输出

```
-- ==> CMAKE_MODULE_PATH = /home/xzh/share/makefile/mysdk/cmake
-- ==> MYSDK_INCLUDE_DIR = /home/xzh/share/makefile/mysdk/include
-- ==> MYSDK_LIBRARY = /home/xzh/share/makefile/mysdk/lib/libMySdk.a
-- ==> MYSDK_FOUND = TRUE
```

比上例子少了 **Found mysdk:**

#### 9. 继续 make

```
->  pwd
/home/xzh/share/makefile/build
->  make
Scanning dependencies of target mysdk_app
[ 50%] Building C object CMakeFiles/mysdk_app.dir/main.c.o
[100%] Linking C executable mysdk_app
[100%] Built target mysdk_app
->  ./mysdk_app
9
-1
->
```

- 只是比上例多了一个 **FIND_PACKAGE(MYSDK QUIET)** 参数的判断逻辑。
- 同样可以传递 **required** 参数
- **FIND_PACKAGE(MYSDK REQUIRED)** 表示这个库是必须链接的，就会走 **FindMYSDK.cmake** 文件中的 **REQUIRED** 参数逻辑

