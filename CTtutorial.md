## 1.Integer Data Type

[Data Types in C - GeeksforGeeks](https://www.geeksforgeeks.org/data-types-in-c/?ref=lbp)

The **integer datatype** in C is used to store the **integer numbers** (any number including positive, negative and zero without decimal part). Octal values, hexadecimal values, and decimal values can be stored in int data type in C. 

- ***\*Range:\****  -2,147,483,648 to 2,147,483,647
- ***\*Size:\****  <font color=red>**4 bytes**</font>
- ***\*Format Specifier:\*\* ** <font color=red>**%d**</font>
- 

### Syntax of Integer

We use[ ***\*int keyword\**** ](https://www.geeksforgeeks.org/int-1-sign-bit-31-data-bits-keyword-in-c/)to declare the integer variable:

```
int var_name;
```

The integer data type can also be used as

1. ***\*unsigned int:\**** Unsigned int data type   is used to store the data values from<font color=red> zero to positive numbers </font>, but it can’t store negative values like signed int.
2. ***\*short int:\**** It is lesser in size than the int by **2 bytes** so can only store values from -32,768 to 32,767.
3. ***\*long int:\**** Larger version of the int datatype so can store values greater than int.
4. ***\*unsigned short int:\**** Similar in relationship with short int as unsigned int with int.

> ***\*Note:\**** The size of an integer data type is compiler-dependent. We can use [**sizeof** operator ](https://www.geeksforgeeks.org/sizeof-operator-c/)to check the actual size of any data type.

### Example of int

```c
// C program to print Integer data types.
#include <stdio.h>

int main()
{
    // Integer value with positive data.
    int a = 9;

    // integer value with negative data.
    int b = -9;

    // U or u is Used for Unsigned int in C.
    int c = 89U;

    // L or l is used for long int in C.
    long int d = 99998L;

    printf("Integer value with positive data: %d\n", a);
    printf("Integer value with negative data: %d\n", b);
    printf("Integer value with an unsigned int data: %u\n",c);
    printf("Integer value with an long int data: %ld", d);

    return 0;
}

```

**Output**

```
Integer value with positive data: 9
Integer value with negative data: -9
Integer value with an unsigned int data: 89
Integer value with an long int data: 99998
```

[Quiz about C Data Types (geeksforgeeks.org)](https://www.geeksforgeeks.org/quizzes/data-types-gq/)







## 2.Character Data Type

### 2.1 Character Data Type in C

Character data type:

-  Allows its variable to **store only a single character**. 

- The size of the character is **1 byte**.

  It stores a single character and requires a single byte of memory in almost all compilers.

- ***\*Range:\**** (-128 to 127) or (0 to 255)
- ***\*Size:\**** 1 byte
- ***\*Format Specifier:\**** %c

### Syntax of char

The ***\*char keyword\**** is used to declare the variable of character type:

```C
char var_name;
```

### Example of char

```C
// C program to print Integer data types.
#include <stdio.h>

int main()
{
    char a = 'a';
    char c;

    printf("Value of a: %c\n", a);

    a++;
    printf("Value of a after increment is: %c\n", a);

    // c is assigned ASCII values
    // which corresponds to the
    // character 'c'
    // a-->97 b-->98 c-->99
    // here c will be printed
    c = 99;

    printf("Value of c: %c", c);

    return 0;
}

```

### Convert Decimal to Binary in C



![Lightbox](D:\OfficeSpace\MarkdownNotes\cstudy\decimal2binary.png)<img src="D:\OfficeSpace\MarkdownNotes\cstudy\bs2-291x300.jpg" alt="Convert (28) into a binary number." style="zoom:80%;" />

```c
// C Program to Convert Decimal Numbers to Binary 

#include <stdio.h> 

// function to convert decimal to binary 
void decToBinary(int n) 
{ 
	// array to store binary number 
	int binaryNum[1000]; 

	// counter for binary array 
	int i = 0; 
	while (n > 0) { 

		// storing remainder in binary array 
		binaryNum[i] = n % 2; 
		n = n / 2; 
		i++; 
	} 

	// printing binary array in reverse order 
	for (int j = i - 1; j >= 0; j--) 
		printf("%d", binaryNum[j]); 
} 

// Driver program to test above function 
int main() 
{ 
	int n = 17; 
	decToBinary(n); 
	return 0; 
}

```



### 2.2 Character Arithmetic in C

Character arithmetic :    **addition, subtraction, multiplication, and division** . 
In character arithmetic **character** **converts into an integer value** to perform the task. For this ASCII value is used.

To understand better let’s take an example.

```C
// C program to demonstrate character arithmetic.
#include <stdio.h>

int main()
{
	char ch1 = 125, ch2 = 10;
	ch1 = ch1 + ch2;
	printf("%d\n", ch1);
	printf("%c\n", ch1 - ch2 - 4);
	return 0;
}

```

**Output**

```shell
-121
y
```

So **%d** specifier causes an integer value to be printed and **%c** specifier causes a character value to printed. But care has to taken that **while using %c specifier the integer value should not exceed 127**. 

Let’s take one more example.

### ***\*Example 2\****

```C
#include <stdio.h>
// driver code
int main(void)
{
	char value1 = 'a';
	char value2 = 'b';
	char value3 = 'z';
	// perform character arithmetic
	char num1 = value1 + 3;
	char num2 = value2 - 1;
	char num3 = value3 + 2;
	// print value
	printf("numerical value=%d\n", num1);
	printf("numerical value=%d\n", num2);
	printf("numerical value=%d\n", num3);
	return 0;
}

```

**Output**

```
numerical value=100
numerical value=97
numerical value=124
```

### ***\*Example 3\****

```C
#include <stdio.h>

int main()
{
	char a = 'A';
	char b = 'B';

	printf("a = %c\n", a);
	printf("b = %c\n", b);
	printf("a + b = %c\n", a + b);

	return 0;
}

```

#### Output

```c
a = A
b = B
a + b = â
```

- Note that in character arithmetic, the characters are treated as integers based on their ASCII code values. For example, the ASCII code for ‘A’ is 65 and for ‘B’ is 66, so adding ‘A’ and ‘B’ results in 65 + 66 = 131, which is the ASCII code for ‘â’.

## 3.Float Data Type

In C programming [float data type](https://www.geeksforgeeks.org/c-float-and-double/) is used to store floating-point values. Float in C is used to store decimal and exponential values. It is used to store decimal numbers (numbers with floating point values) with single precision.

- ***\*Range:\**** 1.2E-38 to 3.4E+38
- ***\*Size:\**** <font color=red>4 bytes</font>
- ***\*Format Specifier:\**** %f

### Syntax of float

The ***\*float keyword\**** is used to declare the variable as a floating point:

```
float var_name;
```

### Example of Float

```c
// C Program to demonstrate use
// of Floating types
#include <stdio.h>

int main()
{
    float a = 9.0f;
    float b = 2.5f;

    // 2x10^-4
    float c = 2E-4f;
    printf("%f\n", a);
    printf("%f\n", b);
    printf("%f", c);

    return 0;
}

```

存储？？





 

## 4.字节序（Endianness）：大端和小端

#### 4.1介绍

**字节序**，也就是字节的顺序，指的是多字节的数据在[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)中的存放顺序；在内存中，数据是以字节（8bit）存储的，当存储 16bit或者 32bit时，就面临着大端 （Big-Endian）存储， 还是小端 （Little-Endian） 存储的问题。



#### 4.2 从阅读习惯看待字节序

<img src="D:\OfficeSpace\MarkdownNotes\cstudy\1730512-20210421150757303-483869849.jpg" alt="img" style="zoom:67%;" />

数字是从左往右读的，这符合人类的普遍阅读习惯。

比如十进制数 `int a =  258`:

- 10进制形式为:	`258`								数位从高到低依次是万，千，百，十，个

- 16进制形式为:	`0x00000102`，						   最左边就是高位字节，最右边是低位字节

-   2进制形式为：   `00000000 00000000 00000001 00000010`    最左边就是高位字节，最右边是低位字节



### 4.3 从内存地址的角度看字节序

首先，你可能需要对内存有一些基本的认识：

1. 一个内存单元可以存储一个字节的内容，因此内存单元也常常被称为字节单元。
2. 一个内存单元可以存储8个比特，即8个二进制数。但是，如果换算成16进制，一个内存单元仅能容纳2个16进制数。

 我们在画**内存示意图**的时候， 一个绿色矩形表示一个内存单元，每一个内存单元都有一个内存地址，方便计算机的处理器找到这块内存单元。
另外，我们也习惯于内存地址将低地址端放在下面，高地址端放在上面。

<img src="D:\OfficeSpace\MarkdownNotes\cstudy\image-20241009091253433.png" alt="image-20241009091253433" style="zoom: 50%;" /><img src="https://pic4.zhimg.com/v2-3e365f1d514b985eaf1cc09a5f36529b_r.jpg" alt="img" style="zoom:67%;" />   



**字节存储顺序：**

以十六进制数 `0x12345678` 为例，当它以**大端字节序**存储在内存中时，低地址端 `0x0000` 存储该数的高位字节 `0x12`；高地址端 `0x0003` 存储的是该数的低位字节 `0x78`。

![img](https://img2020.cnblogs.com/blog/1730512/202104/1730512-20210421152405401-428611369.jpg)

当 `0x12345678` 以**小端字节序**存储在内存中时，低地址端 `0x0000` 存储该数的低位字节 `0x78`；高地址端 `0x0003` 存储的是该数的高位字节 `0x12`。

![img](https://img2020.cnblogs.com/blog/1730512/202104/1730512-20210421152412538-813221407.jpg)

![img](D:\OfficeSpace\MarkdownNotes\cstudy\f1bdc581e332a7a79acc20d51f1fc332.png)

在C语言中，整型数据类型通常使用补码表示法来存储负数。

**计算机中，负数统一采用的补码形式存储。所以变量被赋值负数后，本质存的就是补码，不用再手动转换成补码**

补码表示法是将负数的二进制数按位取反（即0变为1，1变为0），再加1得到的结果。

例如，假设一个无符号8位二进制数01011001表示数值89，那么它的补码为10100111表示数值-89。

以一个有符号的int类型数据为例，如果采用补码表示法存储，首先需要确定该数据类型的位数，例如int类型通常占用4个字节，即32位。

对于一个32位的int类型数据，最高位（即符号位）用来表示正负性，0表示正数，1表示负数。如果该数据为正数，则直接使用二进制表示，如果为负数，则按照补码表示法进行存储。

例如，假设int类型数据-89（即十进制的负89）的二进制表示为10010111，那么它的补码为01101001，最终存储的二进制数为1 01101001。

**什么是补码呢？**
正数的补码就是该正数本身，[负数的补码]是绝对值取反后加1

### 4.4 原码、反码、补码

- 整数的2进制表示方法有三种，即原码、反码和补码
- 2进制有**符号位**和**数值位**两部分；最高位是符号位；0** 表示“**正**”，**1** 表示“**负**”。
- 计算机内存中存储的数据是以**补码**的形式存储的
- ##### 转换规则

  - 正整数的原、反、补码都相同。
  - 负整数的三种表示方法各不相同。
    原码：直接将数值按照正负数的形式翻译成二进制得到的就是原码。
    反码：将原码的符号位不变，其他位依次按位取反就可以得到反码。
    补码：反码+1就得到补码。
- ##### 例子
  

```c
int a = 10;
```

![image-20241013173712871](D:\OfficeSpace\MarkdownNotes\cstudy\image-20241013173712871.png)

```C
int b = -10;
```

![image-20241013173834730](D:\OfficeSpace\MarkdownNotes\cstudy\image-20241013173834730.png)

```c

```





## 5. 位操作

[Complete Reference for Bitwise Operators in Programming/Coding - GeeksforGeeks](https://www.geeksforgeeks.org/complete-reference-for-bitwise-operators-in-programming-coding/)



### 5.1 位操作符

### 

| 操作符 | 名称     | 功能                                 |
| ------ | -------- | ------------------------------------ |
| &      | 按位与   | 两数补码按位相比，有0则为0，同1才为1 |
| 1      | 按位或   | 两数补码按位相比，有1则为1，同0才为0 |
| ^      | 按位异或 | 两数补码按位相比，相同为0，相异为1   |
| ~      | 按位取反 | 对一个补码，把0变1，把1变0           |





```C
int x = 0b10101100;  // 二进制表示
int mask = 0b00001111;  // 低 4 位为 1 的掩码
int result = x & mask;
```

> 对于c语言来说，编译器在处理的时候，会把0开头的数字判定为8进制，0x开头的数字判定为16进制。

```c
int a=017; //八进制， 对应十进制为 15
int b=0x17; //十六进制， 对应整数	23
int c=23; // 十进制	17
```



### 5.2移位操作符

移位操作符是针对**补码**的操作符，他们都直接作用于补码。使用语法如下：

```c
a << x;
a >> x;
//前者是左移，后者是右移，a是被移动的值，x表示移动的位数，这里表示的就是把a的补码左移（右移）x位。
```

##### 左移操作符<<

![Logical left Shift](D:\OfficeSpace\MarkdownNotes\cstudy\Logical-Left-Shift.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/41305ade4eafa01057f20a43012f7c98.png)

对于左移操作符，就是补码右侧补0，左侧丢弃。之后a的补码就会发生改变。第三行的两种形式都可以改变a的值，当然也可以用其它变量来接收a左移后的返回值，那这样a本身就不会发生改变。比如

```c
b = a << 1;
short int y=-10;	// 执行后，内存中： y值为 -10,   (11111111 11110110)
y = y << 12; 		// 执行后，内存中： y值为 24576,    (01100000 00000000)
```

[C语言：位操作符详解_2147483674-CSDN博客](https://blog.csdn.net/fsdfafsdsd/article/details/133440418)

##### 右移操作符>>

​	- 对于无符号数，右边用 0 填充；

	- 对于有符号数，根据符号位填充（正数用 0 填充，负数用 1 填充）。
	- For **unsigned numbers**, the bit positions that the shift operation has vacated are zero-filled. For **signed numbers**, the sign bit is used to fill the vacated bit positions. In other words, if the number is positive, 0 is used, and if the number is negative, 1 is used.
	- <img src="D:\OfficeSpace\MarkdownNotes\cstudy\Logical-Right-Shift.png" alt="Logical Right Shift" style="zoom:67%;" />

 ```c
 unsigned short int y=-10;  // 执行后，内存中： y值为 65526, (11111111 11110110)
 y = y >>3;				   // 执行后，内存中： y值为 8190. (00011111 11111110)
 
 
 short int y=-10;	// 执行后，内存中： y值为 -10,   (11111111 11110110)
 y = y >>3; 			// 执行后，内存中： y值为 -2,    (11111111 11111110)
 
 ```

