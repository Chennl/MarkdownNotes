# C语言内存管理 深度解析

## 内存管理

- C语言是一种强大而灵活的编程语言，为程序员提供了对内存的直接控制能力。

- 这种对内存的控制使得C语言非常灵活，但也带来了更大的责任

-  在C语言中，程序员需要负责内存的分配和释放，否则可能会导致内存泄漏和其他内存管理问题。

- 在C语言中，动态内存的管理函数： `malloc`、`calloc`、`realloc` 和 `free` 等。这些函数位于 `stdlib.h` 头文件中

- C语言中的内存管理涵盖：动态内存分配、内存释放、如何防止内存泄漏等内容。

  

  ## 一、 内存区域划分

  <img src="D:\officspace\MarkdownNotes\cstudy\2bca635b1008c1066de36b09357e6fd9.png" alt="img" style="zoom: 50%;" />

  ![img](D:\officspace\MarkdownNotes\cstudy\c505296760f1dde972109abaacc56c98.png)

  ##### 栈：

  - 在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束时 这些存储单元⾃动被释放。
  - 栈区主要存放运行函数而且分配的**局部变量、函数参数、返回数据、返回地址**等。

  ##### 堆：

  - 堆是用于**动态分配内存**的区域，程序员可以通过**malloc、calloc**等函数手动申请一块指定大小的内存空间，并在使用完毕后手动释放该内存空间。

  - ⼀般由程序员分配释放，若程序员不释放，程序结束时可能由操作系统回收

    ![img](D:\officspace\MarkdownNotes\cstudy\1f3e5e391776ad49d5ede0950286dc11.png)

  ## 二、内存分配方式

  在C语言中，内存分配主要有两种方式：**静态分配**和**动态分配**。下面详细介绍这两种方式及其代码示例。

  1. **静态分配**

  静态分配是指在**编译时确定内存分配**的方式。静态分配的内存通常存在于**数据段和栈区**。

  (1) **全局变量**和**静态变量**

  全局变量和静态变量在程序启动时分配内存，并在整个程序运行期间一直存在。

  ```C
  #include <stdio.h>
  
  // 全局变量
  int globalVar = 10;
  
  void function() {
      // 静态变量
      static int staticVar = 20;
      printf("globalVar: %d, staticVar: %d\n", globalVar, staticVar);
  }
  
  int main() {
      function();
      function();
      return 0;
  }
  ```

  

  (2) 局部变量

  **局部变量**在函数调用时分配内存，在函数返回时释放内存。

  ```C
  #include <stdio.h>
  
  void function() {
      // 局部变量
      int localVar = 30;
      printf("localVar: %d\n", localVar);
  }
  
  int main() {
      function();
      function();
      return 0;
  }
  
  ```

  2. **动态分配**

  动态分配则是在程序运行时根据需要进行的，通过标准库函数如`malloc`、`calloc`、`realloc`和`free`来管理。**动态分配的内存通常存在于堆区**。

  

  ### 1.C语言动态内存分配

<img src="D:\officspace\MarkdownNotes\cstudy\f0ff0f900648160568dd25589f6844f7.png" alt="img" style="zoom: 50%;" />

#### 1.1 `malloc` 函数

`malloc`（memory allocation）函数用于分配指定大小的内存块，并返回该内存块的起始地址。它的原型如下：

```c++
void* malloc(size_t size);
//参数：size 是要分配的内存块的大小，单位是字节。
//返回值：malloc 返回一个指向已分配内存块的指针。如果内存分配失败，返回 NULL。
```

#### 1.2 `calloc` 函数

`calloc`（contiguous allocation）函数用于分配内存，但它与 `malloc` 不同的是，`calloc` 在分配内存后会**初始化内存中的所有字节为零**。它的原型如下：

```C
void* calloc(size_t num, size_t size);
//参数：num 是需要分配的元素个数，size 是每个元素的大小（单位：字节）。
//返回值：calloc 返回指向已分配并初始化为零的内存块的指针。如果内存分配失败，返回 NULL。
```

#### 1.3 `realloc` 函数

`realloc`（reallocation）函数用于**重新调整**之前分配的内存块的大小。它的原型如下：

```C
void* realloc(void* ptr, size_t size);
//参数：ptr 是一个指向已分配内存的指针，size 是需要分配的新内存大小（单位：字节）。
//返回值：realloc 返回一个指向新内存块的指针。如果重新分配失败，返回 NULL，并且原来的内存块保持不变。如果 ptr 为 NULL，realloc 的行为就等同于 malloc。
```

#### 1.4 `free` 函数

`free` 函数用于释放之前使用 `malloc`、`calloc` 或 `realloc` 分配的内存。它的原型如下：

```C
void free(void* ptr);
//参数：ptr 是指向之前分配的内存块的指针。如果 ptr 为 NULL，free 不会执行任何操作。
//返回值：free 没有返回值。
```

#### 2. 内存泄漏与防止

内存泄漏是指程序在运行过程中动态分配了内存空间，但没有及时释放它，导致这些内存空间无法再被访问和使用。内存泄漏会导致程序的内存使用不断增加，最终可能耗尽系统资源。

##### 2.1 内存泄漏的原因

内存泄漏通常发生在以下几种情况下：

**常见的内存泄漏原因**

1. **忘记释放内存**：这是最常见的内存泄漏原因。程序员在使用完动态分配的内存后忘记调用 `free` 函数。
2. **重复释放内存**：多次调用 `free` 函数释放同一块内存会导致未定义行为，可能会引发程序崩溃。
3. **指针覆盖**：在未释放内存的情况下，重新赋值指针，导致原来的内存地址丢失，无法再释放。
4. **递归分配**：在递归函数中分配内存，但没有正确的释放机制，导致内存泄漏。

##### 2.2 防止内存泄漏的方法

1. **确保每个 `malloc`、`calloc` 或 `realloc` 的调用都有相应的 `free`**： 确保每次动态分配内存后，都能在适当的地方释放内存。
2. **避免丢失指针**： 在重新分配内存之前，确保保留原始指针。

```C
ptr = (int*)malloc(sizeof(int));
	if (ptr == NULL) {
	    // 错误处理
	}
	// 重新分配
	int* new_ptr = (int*)realloc(ptr, new_size);
	if (new_ptr == NULL) {
	    free(ptr);  // 如果realloc失败，释放原内存
	} else {
	    ptr = new_ptr;
	}
```

```C
#include <stdio.h>
#include <stdlib.h>

void leaky_function() {
    int *p = (int *)malloc(10 * sizeof(int));  // 分配内存
    if (p == NULL) {
        fprintf(stderr, "Memory allocation failed\n");
        return;
    }

    for (int i = 0; i < 10; i++) {
        p[i] = i * 10;
    }

    // 忘记释放内存
    // free(p);
}

int main() {
    for (int i = 0; i < 1000; i++) {
        leaky_function();  // 每次调用都会导致10个整数的内存泄漏
    }

    return 0;
}
```

**4. 使用内存**[**检测工具**](https://cloud.tencent.com/product/tools?from_column=20065&from=20065)

使用内存检测工具，如 Valgrind，可以帮助检测内存泄漏和非法内存访问等问题。

**安装Valgrind**

在Linux系统上，可以使用以下命令安装Valgrind：

```shell
sudo apt-get install valgrind
```

**使用Valgrind**

1) 编译你的程序（假设程序文件名为 `example.c`）：

```C
gcc -g -o example example.c
```

2) 运行Valgrind：

   ```shell
   valgrind --leak-check=full ./example
   ```

   

## 练习

#### 1.读程序回答问题

```C
int val = 20;    
char arr[10] = {0}; 
```

- 第1行代码： 在_______(堆、栈）空间上开辟四个字节
- 第2行代码： 在_______(堆、栈）空间上开辟10个字节

#### 2 完成代码填空

```C
#include <stdio.h>
#include <stdlib.h>
int main()
{
	int num = 0;
	scanf("%d", &num);
	int* ptr = NULL;
	int i = 0;
	ptr = (int*)malloc(num * sizeof(int));
	if (__________)   //判断ptr指针是否为空
	{	
		for (i = 0; i < num; i++)
		{
			*(ptr + i) = 0;//访问开辟的内存然后赋值为0
		}
	}
	for (i = 0; i < num; i++)
	{
		printf("%d ", *(ptr + i));
	}
	_______________;//释放ptr所指向的动态内存
	ptr = NULL;//是否有必要？
	return 0;
}
```

#### 3.常见的动态内存错误有哪些（                 ）

a. 对NULL指针的解引用操作  

 b. 对动态开辟空间的越界访问  

c.对非动态开辟内存使用free释放

d.使用free释放一块动态开辟内存的一部分  

e.对同一块动态内存多次释放 f. 动态开辟内存忘记释放（内存泄漏）

#### 4. 根据第3题，分析以下属于哪种错误：

##### 4.1 （   ）

```C
void test()
{
	int* p = (int*)malloc(INT_MAX);
	*p = 20;//如果p的值是NULL，就会有问题
	free(p);
}
```

##### 4.2 (   )

```C
void test()
{
	int i = 0;
	int* p = (int*)malloc(10 * sizeof(int));
	if (NULL == p)
	{
		printf("%s\n", strerror(errno));//如果是开辟失败就报错
		return 1;
	}
	for (i = 0; i <= 10; i++)
	{
		*(p + i) = i;//当i是10的时候越界访问
	}
	free(p);
}
```



 ##### 4.3 ( )

```C
void test()
{
	int a = 10;
	int* p = &a;
	free(p);//这样是不行的
}
```



##### 4.4 （   ）

```C
void test()
{
	int* p = (int*)malloc(100);
	p++;
	free(p);//p不再指向动态内存的起始位置
}
```

##### 4.5 (  )

```C
void test()
{
	int* p = (int*)malloc(100);
	free(p);
	free(p); 
}
```

##### 4.6 ( )

```C
void test()
{
	int* p = (int*)malloc(100);
	if (NULL != p)
	{
		*p = 20;
	}
}
int main()
{
	test();
}
```

##### 4.7 **请问运行Test 函数会有什么样的结果？**

```C
void GetMemory(char* p) 
{
	p = (char*)malloc(100);
}
void Test(void) 
{
	char* str = NULL;
	GetMemory(str);
	strcpy(str, "hello world");
	printf(str);
}
```

 ##### 4.8 **请问运行Test 函数会有什么样的结果？**

```C
char* GetMemory(void) 
{
	char p[] = "hello world";
	return p;
}
void Test(void) 
{
	char* str = NULL;
	str = GetMemory();
	printf(str);
}
```

##### 4.8**请问运行Test 函数会有什么样的结果？**

```C
void GetMemory(char** p, int num) 
{
	*p = (char*)malloc(num);
}
void Test(void) 
{
	char* str = NULL;
	GetMemory(&str, 100);
	strcpy(str, "hello");
	printf(str);
}
```

4.9**请问运行Test 函数会有什么样的结果？**

```C
void Test(void) 
{
	char* str = (char*)malloc(100);
	strcpy(str, "hello");
	free(str);
	if (str != NULL)
	{
		strcpy(str, "world");
		printf(str);
	}
}
```

