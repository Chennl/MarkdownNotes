# C++ Object-Oriented Programming 

# 面向对象-翁凯  Spring 2013

### 2. Classes

#### 2.1 What is an object?

- Object = Entity
- Object may be

​	-Visible or
​	-invisible

- Object is variable in programming languages

- Objects = Attributes + Services

  - Data: the properties or status

  - Operations: the functions

    <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241103212349806.png" alt="image-20241103212349806" style="zoom:50%;" />

    灯： 功率、亮还是暗。 操作：通电，断电

    杯： 颜色，容积。 

#### 2.2 Mapping

   - From the problem space to solution space.

     <img src="D:\OfficeSpace\MarkdownNotes\assets\企业微信截图_17306405805527.png" alt="企业微信截图_17306405805527" style="zoom:50%;" />

#### 2.3 Procedural Languages

- C doesn't support relationship btw data and functions.

     ```c
     // C version
     typedef struct point3d {
         float x;
         float y;
         float z;
     } Point3d:
     void Point3d_print(const Point3d* pd);
     Point3d a;
     a.x=1; a.y=3; a.z=3;
     
     Point3d_print(&a);
     ```

```c++
//c++ version
class Point3d{
    public:
        Point3d(float x,float y,float z)
        print();
    private:
        float x;
        float y
        float z;
};
Point3d a(1,2,3);
a.print()
```

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241103214019681.png" alt="image-20241103214019681" style="zoom: 50%;" />

#### 2.4 What is object-oriented
- A way to organize
  - Designs		//找解决问题的思路、方法 是设计
  - Implementations   //  是一个 把思路，方法 用代码写出来  实现过程
- Objects, not control or data flow, are the primary focus of the design and implementation.
- To focus on things, not operations.
- Objects send and receive messages (objects do things!)

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241103214737497.png" alt="image-20241103214737497" style="zoom: 50%;" />

#### 2.5 Objects send messages
- Messages are

  - Composed by the sender
  - Interpreted by the receiver
  - Implemented by methods

- Messages

  - May cause receiver to change state

  - May return results

    老师(sender) 说："某某同学请站起来"(message)， 同学(receiver)听话（message)到后理解（Interpreted ）话的意思，然后，同学站起来也可能 站起来 （Implemented ）。

    程序员写代码，有时候写着写着，就想去直接修改类的代码， 而不通过成员函数去修改。这违背了OOP原则, 即，直接去接触 蛋黄。

#### 2.6 Object vs. Class

- Objects (cat)

  - Represent things, events, or concepts
  - Respond to messages at run-time

- Classes (cat class)

  - Define properties of instances  

  - Act like types in C++

    <img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241103220413539.png" alt="image-20241103220413539" style="zoom:50%;" />

    object 是一个实体，一个东西；   class: 东西的种类, 在C++中就是一个类型，自定义类型。

#### 2.7 OOP Characteristics

1.Everything is an object.
2.A program is a bunch of objects telling each other **what to do** (not how to do) by sending messages
3.Each object has its own memory made up of other objects
4.Every object has a type
5.All objects of a particular type can receive the same messages

例如：对于 2点，餐厅你告诉服务员 只告诉他 你要一杯水，他怎么去倒水，去哪里倒水，你不需要去关注。老师叫你明天交作业，具体如何去做老师不会去管你。

#### 2.8 An object has an interface

- The interface is the way it receives messages

- It is defined in the class the object belong to

  对象以接口和外界接触， 如，灯，接口有 通电接口， 向发光接口；

  ![image-20241103223422024](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241103223422024.png)

  <img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241103222133755.png" alt="image-20241103222133755" style="zoom:50%;" /<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241103222203128.png" alt="image-20241103222203128" style="zoom: 33%;" />

#### 2.8 Functions of the interface

- Communication

- Protection

  灯不会直接伸出两个电线，直接接到家里的电源线， 而是通过接口 方便对接。

#### 2.9 The Hidden Implementation

- lnner part of an object, data members to present its state,and the actions it takes when messages
  is rcvd is hidden.
- Class creators vs. Client programmers
  -Keep client programmers' hands off portions they should not touch.
  -Allow the class creators to change the internal working of the class without worrying about how it will affect the client programmers

#### 2.10 Encapsulation

- bundle data and methods dealing with these data together in an object
- Hide the details of the data and the action
- Restrict only access to the publicized methods.

### 4.  Ticket Machine

- Ticket machines print a ticket when a customer inserts the correct money for their fare.

- Our ticket machines work by customers "inserting" money into them, and then requesting a ticket to be
  printed. A machine keeps a running total of the amount of money it has collected throughout its operation.
  
  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104130951861.png" alt="image-20241104130951861" style="zoom:67%;" />

#### 4.1 Procedure-Oriented

- Step to the machine
- Insert money into the machine
- The machine prints a ticket
- Take the ticket and leave

#### 4.2 Something is here

![image-20241104102719541](D:\OfficeSpace\MarkdownNotes\assets\image-20241104102719541.png)

 <img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241104102921401.png" alt="image-20241104102921401" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241104103120128.png" alt="image-20241104103120128" style="zoom:50%;" />

+方法

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104110222592.png" alt="image-20241104110222592" style="zoom:50%;" />

### 4.3 :: resolver

- \<Class Name>::\<function name>
- ::\<function name>

void S::fC) {
	::f(); 	// Would be recursive otherwise!
	::a++; 	// Select the global a
	a--; 	// The a at class scope



### 5.头文件

#### 5.1 Definition of a class

- In C++,separated .h and .cpp files are used to **define** one class.
- Class **declaration**  and prototypes in that class are in the header file (.h).
- All the bodies of these functions are in the source file (.cpp)

```c
void fun();	 //声明
int global;  //定义
extern int global;  //声明

```



#### 5.2 Header = interface
- The header is a contract between you and the user of your code.
- The compile enforces the contract by requiring you to declare all structures and  functions before they are used.

#### 5.3 C++ Structure of C++ program

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241103203046521.png" alt="image-20241103203046521" style="zoom: 50%;" />

#### 5.4 Declarations vs. Definitions 
- A .cpp file is a compile unit 
- Only **declarations** are allowed to be in .h
  - extern variables	有 extern 变量是声明， 没有的是定义
  - function prototypes  函数原型， 即，没有 { }是声明，有 { }是定义
  - class/struct declaration 类和结构 只有声明，没有定义

#### 5.5 #include
- **#include** is to insert the included file into the .cpp file at where the #include statement is.

- #include “xx.h" :  first search in the current directory then the directories declared somewhere
- #include <xx.h>: search in the specified directories
- #include <xx>: same as #include <xx.h>

#### 5.6 Standard header file structure
~~~c++
#ifndef HEADER_FLAG
#define HEADER_FLAG
// Type declaration here..
#endif // HEADER_FLAG
~~~

```c++
#ifndef BOX_H   //如果没定义宏 ， 则，把以下代码编译进去，否则，编译时，不把以下代码编译进去
#define BOX_H 	//定义宏，用来表示 这个代码被编译过

class Box {
public:
    // 构造函数
    Box(double length, double width, double height);
    // 析构函数
    ~Box();
    // 计算体积
    double getVolume() const;
private:
    double length; // 长度
    double width;  // 宽度
    double height; // 高度
};

#endif // BOX_H
```

#### 5.7 Tips for header

1. One class declaration per header file
2. Associated with one source file in the same prefix of file name.
3. The contents of a header file is surrounded with **#ifndef #define #endif**

### 6.  Clock Display

![image-20241104132806914](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241104132806914.png)

用c ++实现时钟。 用C写面向过程程序很简单。  但， 我们c++OOP思想去实现，关注什么东西，不关注过程是怎么样的。

#### 6.1 Abstract

- Abstraction is the ability to ignore details of parts to focus attention on a higher level of
  a problem.

  看到人，你看到衣服，身高，不会关注人的内部消化系统，循环系统灯

  看到大街上汽车， 牌子，颜色，外形， 而不会关注 发动机、排气管。 

  时钟： 小时，分钟

- Modularization is the process of dividing a whole into wel-defined parts, which can be built and examined separately, and which interact in well-defined ways.

#### 6.2 Modularizing the clock display：

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104133721473.png" alt="image-20241104133721473" style="zoom:67%;" />

#### 6.3 Object & Classes

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104133853786.png" alt="image-20241104133853786" style="zoom: 50%;" />

#### 6.5 Class Diagram

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134038014.png" alt="image-20241104134038014" style="zoom: 50%;" />

#### 6.6 Implementation ClockDisplay 



<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134723597.png" alt="image-20241104134723597" style="zoom:50%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134828530.png" alt="image-20241104134828530" style="zoom:50%;" />



? 若要上面程序跑起来，合理地需要多少个文件？

### 7. 成员变量

#### 7.1 local variable - 本地变量 

- Local variables are defined **inside** a method, have a **scope** limited to the method to which they belong

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104135145939.png" alt="image-20241104135145939" style="zoom: 67%;" />

balance: 成员variable 	amountToRefund:  local variable

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104135408115.png" alt="image-20241104135408115" style="zoom: 67%;" />

#### 7.2 Fields,parameters,local variables

- All three kinds of variable are able to store a value that is appropriate to their defined type.
- **Fields** are defined outside constructors and methods 
- Fields are used to store data that persists throughout the life of an object. As such, they maintain the current state of an object. They have **a lifetime that lasts as long as their object lasts.**
- Fields have class scope:their accessibility extends throughout the whole class, and so they can be used within any of the constructors or methods of the class in which they are defined.

成员变量属于 变量，不属于类；成员函数属于类，不属于变量;

#### 7.3 Call functions in a class

```c 
Point a;
a.print();
```

- There is a relationship with the function be called and the variable calls it.
- The function itself knows it is doing something with the variable.

#### 7.4 this:the hidden parameter

**this** is a hidden parameter for all member functions. with the type of the class

``` c++
void Point::print()
>(can be regarded as)
void Point::print(Point *p)
```

### 9 构造与析构函数

```C++
class Point{
    public:
        void init(int x,int y);
        void print() const;
        void  move(int dx,int dy);
    private:
        int x;
        int y;
}
Point a;
a.init(1,2)
a.move(2,2)
a.print0;
```

### 9.1 Guaranteed initialization with the constructor

- lf a class has a constructor, the compiler automatically calls that constructor at the point an object is created, before client programmers can get their hands on the object.

- The name of the constructor is the same as the name of the class.
- With no return type

### 9.2 How Constructor does?

```c++
class X{
    int i;
    public:
    X();
};
```

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104202152115.png" alt="image-20241104202152115" style="zoom:50%;" />

#### 9.4 Constructors with arguments

- The constructor can have arguments to allow you to specify how an object is created, give it initialization values, and so on

  ```c++
  Tree(int i) {....}
  ~Tree(){...}
  
  
  
  class Tree {
  	int height;
  	public:
          Tree(int initialHeight);           // Constructor
          ~Tree(); // Destructor
          void grow(int years);
          void printsize();
  }
      
  Tree::Tree(int initialHeight) {
      height = initialHeight;
      cout << "inside Tree::Tree()" << endl;
  }
  
  Tree t(12);
  ```

  

- Constructor1.cpp

#### 9.5 The destructor

- In C++,cleanup is as important as initialization and is therefore guaranteed with the destructor.

- The destructor is named after the name of the class with a leading tilde (~).

- The destructor **never** has any arguments

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104203301056.png" alt="image-20241104203301056" style="zoom:67%;" />

  收回变量空间

```c++
~Tree(){...}

Tree::~Tree() {
cout << "inside Tree destructor" << endl;
printsize();
}


int main() {

    cout << "before opening brace" << endl;
    {
        Tree t(12);
        cout << "after Tree creation" << endl;
        t.printsize();
        t.grow(4);
        cout << "before closing brace" << endl;
    }//括号内变量释放空间
    cout << "after closing brace" << endl;
}
```

#### 9.6 When is a destructor called?

- The destructor is called automatically by the compiler when the object goes out of scope.
- The only evidence for a destructor call is the **closing brace** of the scope that surrounds the object.

{  **}** 出这个括号

### 10.Storage allocation

- The compiler allocates all the storage for a scope at the **opening brace** of that scope.
- The constructor call doesn't happen until the sequence point where the **object is defined**.
  - Examlpe: Nojump.cpp

```c++
// (c) Bruce Eckel 2000
// Copyright notice in Copyright.txt
// Can't jump past constructors
class X {
    public:
    x();
};
X::X(){ }
void f(int i) {
    if(i < 10) {
    //! goto jump1; // Error: goto bypasses init
    }
    X x1;
    // Constructor called here
    jump1:
    switch(i) {
        case 1 :
            X x2;
            // Constructor called here
            break;
        //! case 2 : // Error: case bypasses init
        X x3; // Constructor called here
        break;
    }
}
```

#### 10.1 Aggregate initialization

```c++
int a[5] = [ 1, 2,3,4, 5};
int b[6] = f{5}:
int c[] = [1,2,3,4];
// sizeof c / sizeof *c
struct X { int i; float f; char c; };
//X xl={1,2.2,'c'};
X x2[3] = { {1，1.1,'a'}，{2，2.2,'b'};
struct Y {float f; int i; Y(int a); };
 //结构 有构造函数，需要调用构造函数初始化
Y y1[] = { Y(1)，Y(2)，Y(3)};
```

#### 10.2 The default constructor

- A default constructor is one that can be called with no arguments.

```c++
class A{
    int i;
    public:
    A(int a);
}
A::A(int a){
    this->i=a;
}

int main(){
    A a[2]={A(1)};  //编译时会报错，什么错误呢？
    A aa; //报错
}
```

### 11. Dynamic memory allocation

- new
  - new int;
  - new Stash;    //分配空间，然后调用 构造函数，最后，返回地址
  - new int[10]
- delete
  - delete p;
  - delete  p[ ];

#### 11.1 new and delete

- new is the way to allocate memory as a program runs.Pointers become the only
  access to that memory
- delete enables you to return memory to the memory pool when you are finished with it.
  Example:use_new.cpp

#### 11.2 Dynamic Arrays

int *psome = new int [10];

- The new operator returns the address of the first element of the block

delete [] psome;

- The presence of the brackets tells the program that it should free the whole
  array, not just the element 
  Example:arraynew.cpp
