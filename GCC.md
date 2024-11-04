# GCC

[GCC and Make - A Tutorial on how to compile, link and build C/C++ applications](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html) 

### <font color=#0c9b74 > 1. Install MinGW-w64 GCC Compiler</font>

MinGW-w64 (short for "Minimalist GNU for Windows") (@ https://www.mingw-w64.org/), is a minimalist  development environment for native Microsoft Windows applications, in particular:

To install MinGW-w64:

1. You can install MinGW-w64 via MSYS2 (@ https://www.msys2.org/), which provides up-to-date native builds of GCC, Mingw-s64 and other helpful C++ tool and libraries..

2. Add "`<MINGW_HOME>/bin`" to PATH where `<MINGW_HOME>` is the MinGW installed directory.

3. Verify the GCC installation by listing the version of `gcc,` `g++` and `gdb`

   ```shell
   > gcc --version
   gcc (x86_64-win32-seh-rev0, Built by MinGW-Builds project) 14.2.0
   ......
    
   > g++ --version
   g++ (x86_64-win32-seh-rev0, Built by MinGW-Builds project) 14.2.0
   ......
    
   > gdb --version
   GNU gdb (GDB) 14.2
   ......
   ```

#### <font color=#0c9b74 >2 Getting Started</font>

The GNU C and C++ compiler are called `gcc` and `g++`, respectively.

##### 2.1 Compile/Link a Simple C Program - hello.c

Below is the Hello-world C program `hello.c`:

```C
// hello.c
#include <stdio.h>
 
int main() {
    printf("Hello, world!\n");
    return 0;
}
```



```c
> gcc hello.c
  // Compile and link source file hello.c into executable a.exe (Windows) or a (Unixes)
```

The default output executable is called "`a.exe`" (Windows) or "`a.out`" (Unixes and Mac OS X).

To run the program:

```C
// (Windows) In CMD shell
> a.exe
```

To specify the output filename, use `-o` option:

```c
// (Windows) In CMD shell
> gcc -o hello.exe hello.c
  // Compile and link source file hello.c into executable hello.exe
> hello
  // Execute hello.exe under CMD shell
```

##### 2.2 Compile/Link a Simple C++ Program - hello.cpp

You need to use g++ to compile C++ program, as follows. We use the `-o` option to specify the output file name.

```c++
// hello.cpp
#include <iostream>
using namespace std;
 
int main() {
   cout << "Hello, world!" << endl;
   return 0;
}
```

You need to use g++ to compile C++ program, as follows. 

We use the `-o` option to specify the output file name.

```c
// (Windows) In CMD shell
> g++ -o hello.exe hello.cpp
   // Compile and link source hello.cpp into executable hello.exe
> hello
   // Execute under CMD shell
```



##### 2.3 Compile and Link Separately



The above command *compile* the source file into object file and `link` with other object files and system libraries into executable in one step. You may separate compile and link in two steps as follows:

```c
// Compile-only with -c option
> gcc -c -Wall -g Hello.c
// Link object file(s) into an executable
> gcc -g -o Hello.exe Hello.o
```

```c
// Compile-only with -c option
> g++ -c -Wall -g Hello.cpp
// Link object file(s) into an executable
> g++ -g -o Hello.exe Hello.o
```





##### 2.4 More GCC Compiler Options

A few commonly-used GCC compiler options are:

```
$ g++ -Wall -g -o Hello.exe Hello.cpp
```

- `-o`: specifies the output executable filename.
- `-Wall`: prints "`all`" Warning messages.
- `-g`: generates additional symbolic debugging information for use with `gdb` debugger.

#### 2.4 GCC Compilation Process

![img](D:\OfficeSpace\MarkdownNotes\cstudy\GCC_CompilationProcess.png)

GCC compiles a C/C++ program into executable in 4 steps as shown in the above diagram.

 For example, a "`gcc -o hello.exe hello.c`" is carried out as follows:

1. Pre-processing: via the GNU C Preprocessor (cpp.exe), which includes the headers (#include) and expands the macros (#define)

   ```c
    > cpp hello.c > hello.i
   //> gcc -E hello.c  -o hello.i
   ```

   The resultant intermediate file "hello.i" contains the expanded source code.

2. Compilation: The compiler compiles the pre-processed source code into assembly code for a specific processor.

   ```c
   > gcc -S hello.i
   ```

   The  **-S**  option specifies to produce assembly code, instead of object code. The resultant assembly file is`hello.s`".

3. Assembly: The assembler (as.exe) converts the assembly code into machine code in the object file "hello.o".

   ```c
   > as -o hello.o hello.s
   ```

4. Linker: Finally, the linker (ld.exe) links the object code with the library code to produce an executable file "hello.exe".

   ```c
   >gcc  hello.o -o hello
   
   > ld -o hello.exe hello.o ...libraries...  ??
   ```

##### 

 

# CMake 

在 [gcc小节](https://blog.csdn.net/u011895157/article/details/131052194#gcc) 中给出了程序生成可执行文件的流程，但是如果程序很多的时候，这种方式就显得太繁琐了。因此就需要用到 makefile，makefile 是一种能够自动化构建和编译项目的文本文件。简单来说只要存在 makefile文件， make 一下就能完成项目的编译工作。

CMake 能够解决该问题，它能跟据不同的编译平台，**自动**生成 Makefile 文件:

![img](D:\OfficeSpace\MarkdownNotes\assets\c9245cf4c2f0baac835c63ec1fa7e4b2.png)

### **构建工程项目流程**

（1）构建CMakeLists.txt

CMake 构建脚本是一个纯文本文件，必须将其命名为 CMakeLists.txt，并在其中包含 CMake 构建您的 C/C++ 库时需要使用的命令。[【C++】Cmake使用教程（看这一篇就够了）-CSDN博客](http://www.360doc.com/content/24/0411/11/63953942_1120066798.shtml) 

- 单个文件: 

  ```yaml
  tutorial/   #项目目录
  	CMakeLists.txt
  	tutorial.cpp
  ```

  

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

add_executable(tutorial   tutorial.cpp)
 
```

- 多个文件：

```cmake
 tutorial/   #项目目录
 	CMakeLists.txt
	tutorial.cpp
	box.cpp
	box.h
```

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

#方法1：对于多个文件 都罗列进去
  add_executable(tutorial  box.cpp box.h tutorial.cpp)
 
#方法2： 
# 通过aux_source_directory 当前目录下的源文件存列表存放到变量SRC_LIST
aux_source_directory(. SRC_LIST) 
add_executable(tutorial ${SRC_LIST})

#方法3： 
# 当前目录下的源文件存列表存放到变量SRC_LIST, 避免不需要的文件也包含进去
set(SRC_LIST 
	./tutorial.cpp 
	./box.cpp 
	./box.h 
	)
add_executable(tutorial ${SRC_LIST})

```

- 子目录：

```cmake
 #项目目录
 tutorial/ 
 		CMakeLists.txt
	 	src/	#源代码目录
            tutorial.cpp
            box.cpp
            student.cpp
            box.h
            student.h
          build/ 	#可执行文件输出目录
          	tutorial.exe  
```

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

# 通过aux_source_directory 当前目录下的源文件存列表存放到变量SRC_LIST
# 通过include_directory 当前目录下的源文件存列表存放到变量SRC_LIST
set(SRC_LIST 
	./tutorial.cpp 
	./src/box.cpp
	./src/student.cpp
	)

add_executable(tutorial ${SRC_LIST})
```

（2）vscode 插件 ctrl+shift+P    

​	CMake: configue

 (3) vscode 插件 ctrl+shift+P       

​	 CMake:build

(4) 在build 目录，输出编译好的  tutorial.exe
