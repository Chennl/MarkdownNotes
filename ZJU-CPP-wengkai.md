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

void S::f( ) {
	::f(); 	// Would be recursive otherwise!
	::a++; 	// Select the global a
	a =a- 1; 	// The a at class scope

}

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

用c ++实现时钟。 用C写面向过程程序很简单。  但， 我们c++ OOP思想去实现，关注什么东西，不关注过程是怎么样的。

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

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134038014.png" alt="image-20241104134038014" style="zoom: 33%;" />

#### 6.6 Implementation ClockDisplay 



<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134723597.png" alt="image-20241104134723597" style="zoom: 33%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104134828530.png" alt="image-20241104134828530" style="zoom: 33%;" />



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

#### 11.3 The new-delete mech.

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104213643116.png" alt="image-20241104213643116" style="zoom:50%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241104213850545.png" alt="image-20241104213850545" style="zoom:67%;" />



​		区别：

​		delete r;    析构函数只调用一个，但空间都释放了

​		delete[] r;  析构函数都调用一个，空间也都释放了

```C++
#include <iostream>
using namespace std;
class A
{
    private:
        int i;

    public:
        A()
        {
            i = 0;
            cout << "A::A()" << endl;
        }
        ~A()
        {
            cout << "A::~A(),i=" << i << endl;
        }
        void set(int i)
        {
            this->i = i;
        }
        void f()
        {
            cout << "hello";
        }
};

int main()
{
	A* p = new A[10];
	for ( int i=0; i<10; i++ )
		p[il.set(i);
	delete []p;
    // delete p;   
	return 0;
          }
```



#### 11.4 Tips for new and delete

- Don't use **delete** to free memory that **new** didn't allocate
- Don't use **delete** to free the same block of memory twice in succession.
- Use **delete []** if you used **new []** to allocate an array
- Use delete (no brackets) if you used new to allocate a single entity
- lt's safe to apply **delete** to the null pointer (nothing happens)

### 12. Setting limits

- to keep the client programmer's hands off members they shouldn't touch.
- to allow the library designer to change the internal workings of the structure without worrying about how it will affect the client programmer

#### 12.1 C++ access control

The members of a class can be cataloged, marked as:

- public

  **public** : means all member declarations that follow are available to everyone.

- private

  The **private** keyword means that **no one** can access that member except **inside function members** of that type.

  

  private 以对类来说的，不是对变量来说的，同一个类的不同对象(变量)，可以访问私有成员.

  ![企业微信截图_17307832509539](C:\Users\chennl\AppData\Local\Temp\企业微信截图_17307832509539.png)

  注意： **p->i** 可以被访问。

  这些是在编译时刻检查，运行时刻没有限制

- protected

### 12.2 Friends

<<<<<<< HEAD


什么是**友元函数**   -- 朋友函数
在C++中，友元函数是一种特殊的函数，它被声明为一个类的友元(friend)，允许该函数访问、修改类的私有成员。

友元函数的声明通常放在**类**的声明中，但并**不属于**类的**成员函数**。

一个类，指定了某一个函数 是自己的朋友，那么这个朋友就有权访问这个类里面的机密数据了。好东西只有好朋友才能享用

![image-20241205214609054](D:\OfficeSpace\MarkdownNotes\assets\image-20241205214609054.png)

- 1.2.友元类
  除了友元函数，C++还支持友元类。友元类是指一个类的成员函数可以访问另一个类的私有成员



友元函数提供了在需要访问类的私有成员时的一种灵活机制，但应**慎重使用**，因为过多的友元可能**破坏**封装性和类
的独立性。

2友元函数的特点和注意事项
2.1**友元函数不是类的成员函数**
友元函数不是成员函数，也就是说他不是类的成员，而只是拥有了类的通行许可证。如果，强制在类的内
部进行定义了友元函数，编译器会把他变成内联函数来看待
也就是说:强调友元函数的独立性，不受类的成员函数的约束。虽然友元函数可以访问类的私有成员，但
它不是类的一部分，也没有隐式的 this 指针，因此在语法和概念上与成员函数有所不同

2.2友元关系不具有传递性
如果A是B的友元，B是C的友元，那并不意味着A是C的友元。
友元，不但存在友元函数也存在友元类

2.3，友元关系是单向的
如果A是B的友元，不一定意味着B是A的友元

2.4，友元函数可以是全局函数或其他类的成员函数
不仅可以将非成员函数声明为类的友元，还可以将其他类的成员函数声明为友元。 这个就是说:
两个类可以共同拥有一个友元函数。



2.5友元函数的声明位置
友元函数的声明通常放在类的 public、 private或protected 部分中。这是为友元关系不受访问控制符
的限制，它是在类的声明中建立的。



=======
>>>>>>> 2d306f190c2d4a59a28fb92f4e52e57ecfa47cdd
- to explicitly grant access to a function that isn't a member of the structure

- The class itself controls which code has access to its members.

- Can declare a global function as a **friend**, as well as a member function of another class, or even an entire class, as a **friend**.

  ```c++
  //: C05:Friend.cpp
  // From Thinking in C++, 2nd Edition
  // Available at http://www.BruceEckel.com
  // (c) Bruce Eckel 2000
  // Copyright notice in Copyright.txt
  // Friend allows special access
  // Declaration (incomplete type specification)
  struct X;
  struct Y {
  void f(X*);
  }
  struct X  // Definition
      private:
          int i;
      public:
      void initialize();
          friend void g(x*，int); // Global friend
          friend void Y::f(X*); // Struct member friend
          friend struct Z; // Entire struct is a friend
          friend void h();
  }
  
  void X::initialize() {
  	i=0;
  }
  void g(X* x, int i) {
  	X->i = i;
  }
  void Y::f(xX* x) {
  	x->i = 47;
  }
  ```

  

  

  ### 13. Initializer list

  

  ```c++
  class Point {
      private:
          const float x,y;
          Point(float xa = 0.0, float ya = 0.0):y(ya)，x(xa){
              
          }
  };
  ```

- Can initialize any type of data

  - pseudo-constructor calls for built-ins
  - No need to perform assignment within body of ctor

- Order of initialization is order of declaration

  - Not the order in the list!
  -  Destroyed in the reverse order.

  #### 13.1 lnitialization vs.assignment

  ````c++
  Student::Student(string s):name(s){ }
  // initialization
  // before constructor
      
  Student::Student(string s)
  {name=s;}
  
  //assignment
  // inside constructor
  // string must have a default constructor
  ````

  

  ### 14. Reusing the implementation

  - Composition: construct new object with existing  objects
  - lt is the relationship of“has-a'

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241105215641936.png" alt="image-20241105215641936" style="zoom:50%;" />

  

  Objects can be used to build up other objects

- Ways of inclusion:

  - Fuly		//定义对象
  - By reference     //指针形式

- Inclusion by reference allows sharing

  For an example:

  Employee has a

  -Name

  -Address
  -Health Plan

  -Salary History
  	Collection of Raise objects

  -Supervisor
   Another Employee object!

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241105220720394.png" alt="image-20241105220720394" style="zoom:50%;" />

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241105220841098.png" alt="image-20241105220841098" style="zoom: 50%;" />

  

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241105221105680.png" alt="image-20241105221105680" style="zoom:50%;" />

#### 14.2 Embedded objects

- All embedded objects are initialized

  - The default constructor is called if
  you don't supply the arguments, and there is a default
  constructor (or one can be built)

- Constructors can have initialization list

  - any number of objects separated by commas
  - is optional
  - Provide arguments to sub-constructors

- Syntax:

  name( args ) [ ':' init-list ] '{' 

#### 14.3 Question

- lf we wrote the constructor as (assuming we have the set accessors for the subobjects):

``` c++
class SavingsAccount {
public:
	Person m saver;
    ...
}  //assume Person class has set_name()

SavingsAccount::SavingsAccount ( const char* name
const char* address, int cents ) {
    m saver.set_name( name );
    m saver.set_address( address );
    m balance.set_cents( cents );
}
```

- Default constructors would be called;



#### 14.4 Public Vs.Private

- lt is common to make embedded objects private:

  - they are part of the underlying implementation
  - the new class only has part of the public interface of the old class

- Can embed as a public object if you want to have the entire public interface of the subobject available in the new object:

  ```c++
  class SavingsAccount {
  public:
  	Person m saver;
      ...
  }  //assume Person class has set name()
  SavingsAccount  account;
  account.m saver.set name("Fred" ); //这样违反 OPP鸡蛋模型
  ```

  


### 15.  Reusing the interface

- Inheritance is to take the existing class, clone it. and then make additions and modifications to the clone.

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241106104754002.png" alt="image-20241106104754002" style="zoom: 67%;" />

#### 15.1 Inheritance

- Language implementation technique

- Also an important component of the OO design methodology

- Allows sharing of design for
  -Member data
  -Member functions
  -**Interfaces** 

- Key technology in C++

- The ability to define the behavior or implementation of one class as a **superset** of
  another class

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241106105641482.png" alt="image-20241106105641482" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241106143927315.png" alt="image-20241106143927315" style="zoom:50%;" />

#### 15.2 public private and protected

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241106161115989.png" alt="image-20241106161115989" style="zoom:50%;" />

private: 子类不能使用, 外部不能使用

protected: 自己和子类内部可以访问，外部不能使用

```c++
#include <iostream>
using namespace std;


class A
{
    private:
        int i;
    protected:
    	int a;
 
    	
    public:
        A():i(0){
            cout << "A::A()" << endl;
        }
        ~A(){
            cout << "A::~A(),i=" << i << endl;
        }
        void print(){
            cout << "A::print(),i=" << i << endl;
        }
    protected:
        void set(int ii){
            this->i =ii;
        }
    private:
    	void msg(){
            cout <<"hello";
        }
};
class B: public A{
    public:
        void f(){
            //protected 子类可以访问 
                set(20);  
            //private 子类不能访问
            //error: 'void A::msg()' is private within this context
               msg();
            
 			//private 子类不能访问
          	 // error: 'int A::i' is private within this context
            // note: declared private here
              i=30;
            // protected 子类可以访问
              b=20;
              print();

            }
};
 
int main(){

    B b;
    b.print();
 	b.f();
    
    //protected 外部不能访问
    //error: 'void A::msg()' is private within this context
    b.set(100);
    
	//private 子类不能访问
    //error: 'void A::msg()' is private within this context
    b.msg();
    
    


    return 0;
}

```





若在 A::set() 变成 protected, 则：

```c++
    protected:
        void set(int ii){
            this->i =ii;
        }
//则：
int main(){
    B b;
    // private, protected 外部不能访问
   // error: 'void A::set(int)' is protected within this context
    b.set(100);
    b.msg();

        
    b.print();
    b.f();
    return 0;
}
```



#### 15.4 Declare an Employee class

```C++
class Employee {
    public:
        Employee( const std::string& namer, const std::string& ssn );
        const std::string& get_name () const;
 
        void print(std::ostream&  out) const;
        void print(std::ostream&  out, const  std::string& msg) const;
    protected:
        std::string m name;
        std::string m ssn;
};
//2. Constructor for Employee
Employee::Employee( const string& name, const string& ssn ):name(name),ssn(ssn){
// initializer list sets up the values!
}

//3. Employee member functions
inline const std::string& Employee::get name() const{
return m name;
}
inline void Employee::print( std::ostream& out )const {
    out << m_name << endl;
    out << m_ssn << endl;
}
inline void Employee::print(std::ostream& out, const,std::string& msg) const {
    out << msg << endl;
    print(out);//调用已有代码， 避免代码 code duplicate
}

// 4. Now add Manager
class Manager : public Employee {
    public:
    	Manager(const std::string& name,const std::string& ssn,
    			const std::string& title);
        const std::string title_name() const;
        const std::string& get_title() const;
        void print(std::ostream& out) const;  //和Employee  print(out)一样，那会怎么样？
    private:
    	std::string m title;//扩展属性
}
```

#### 15.5 Inheritance and constructors

- Think of inherited traits as an embedded object

- Base class is mentioned by class name

  ```c++
  Manager::Manager( const string& name， const string&
  ssn, const string& title = "" ):Employee(name, ssn), m title( title ){
      
  }
  ```

  先 父类A  constructors()  , 然后 子类B  constructors（）

  先 子类B  destory()  , 然后 父类A destory（）

 ```c
 A::A() 15 0    
 B::B()
 B::~B()        
 A::~A(),i=20 
 ```

#### 15.6 Manager member functions

```C++
inline void Manager::print( std;:ostream& out ) const {
    Employee::print( out );    //call the base class print
    out << m_title << endl;
}
inline const std::string& Manager::get_title() const{
	return m_title;
}
inline const std::string Manager::title_name() const{
return string(m_title +":"+m_name );//access base m_name
}
```



#### 15.7 Use

```c++
int main (){

	Employee bob("Bob Jones"，"555-44-0000");
	Manager bill("Bill Smith"，"666-55-1234"，"Important Person");
	string name = bill.get_name();// okay Manager inherits Employee
	//string title = bob.get title();    // Error -- bob is an Employee!
	cout << bill.title_name() <<  "\n" << endl;
	bill.print(cout);
	bob.print(cout);
	bob.print(cout,"Employee:");
	//bill.print(cout，"Employee:");// Error hidden!
  }
```

C++特别：  子类中有同名的 函数， 则父类中同名函数就被隐藏掉

### 16.Function overloading

- Same functions with different arguments list.
  ```C++
  void print(char *str, int width); // #1
  void print(double d，int width); // #2
  void print(long l，int width); // #3
  void print(int i, int width); //#4
  void print(char *str);// #5
  print("Pancakes"，15);
  print("Syrup");
  print(1999.0，10);
  print(1899L 15);
  ```

  

#### 16.1 Overload and auto-cast
```C++
void f(short i);
void f(double d);
f('a');
f(2);
f(2L);
f(3.2);
```

#### 16.2 Default arguments

- A default argument is a value given in the declaration that the compiler automatically
  inserts if you don't provide a value in the function call.

```C++
Stash(int size, int initQuantity = 0);
```

#### 16.3

- To define a function with an argument list, defaults must be added from right to left.

  ```C++
  int harpo(int n, int m = 4,int j= 5);
  int chico(int n,int m = 6,int j);//illeagle
  int groucho(int k = 1,int m = 2,int n = 3);
  beeps = harpo(2);
  beeps = harpo(1,8);
  beeps = harpo(8,7,6):
  ```

  默认参数数值写在 .h文件中，不能写在 .cpp中。默认值在编译阶段，在编译时，编译器自动补上 参数.

  不建议使用default value, 会被恶意修改



### 17.内联函数

#### 17.1 Overhead for a function call

- the processing time required by a device prior to the execution of a command
  - Push parameters
  - Push return address
  - Prepare return values
  - Pop all pushed

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107111912428.png" alt="image-20241107111912428" style="zoom:50%;" />



#### 17.2 Inline Functions

- An inline function is expanded in place, like a preprocessor macro, so the overhead of the
  function call is eliminated.

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107112511802.png" alt="image-20241107112511802" style="zoom:50%;" />



```c++
inline int plusOne(int x);
inline int plusOne(int x) freturn ++x; };
```

- Repeat **inline** keyword at declaration and definition.
- An inline function definition may not generate any code in .obj file.

可执行代码中没有这个函数， 这个函数被替换程代码

#### 17.4 Inline functions in header file

- So you can put inline functions' bodies in header file.Then #include it where the function is needed
- Never be afraid of multi-definition of inline functions, since they have no body at all.
- Definitions of inline functions are just declarations.

```C++
//a.h
#include <iostream>
using namespace std;
inline void f()
{     cout<< " inline.h"<<endl; }

// main.cpp
#include "inline.h"
 
int main(){
    f();
}


// main.i
...
# 2 "inline.h" 2

# 2 "inline.h"
using namespace std;
inline void f()
{ cout<< " inline.h"<<endl; }
# 2 "inline.cpp" 2

int main(){
    f();
}

```

对于 inline function 只要 a.h ，不需要a.cpp

#### 17.4  Tradeoff of inline functions

- Body of the called function is to be inserted into the caller.
- This may expand the code size but deduces the overhead of calling time.
- So it gains speed at the expenses of space.
- In most cases,it is worth
- lt is much better than macro in C.lt checks the types of the parameters.

空间换时间  

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107195018672.png" alt="image-20241107195018672" style="zoom:50%;" />



#### 17.5 Inline may not in-line

- The compiler does not have to honor your request to make a function inline. lt might decide the function is too large or notice that it calls itself (recursion is not allowed or indeed possible for inline functions), or the feature might not be implemented for your particular compiler.

#### 17.6 Inline inside classes

- Any function you define inside a class declaration is automatically an inline.

  ```c++
  #include <string>
  using namespace std;
  class Point {
      int i, j, k;
      public:
      Point() { i=j=k=0;I
      Point(int ii, int jj, int kk) { i=ii,j=jj,k=kk;}
      void print(string& msg = "") {
          if(msg.size() != 0)  cout << msg << endl;
          cout <<"i =" "<< endl;
      }
  int main() {
  	Point p，q(1,2,3);
  }
  ```

#### 17.6 Access functions

- They are small functions that allow you to read or change part of the state of an object - that is, an
  internal variable or variables.

  ```c++
  class Cup {
      int color;
      public:
     	 int getColor() { return color; }
      void setColor(int color) {   	 
     	 this->color =color;
      }
  ```

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107195923032.png" alt="image-20241107195923032" style="zoom:50%;" />

这样，代码又在 .h文件，又不在class { }内， 这样，保持.cpp样式， 成员函数清晰可见。



#### 17.7 Inline or not?

- Inline:
  - Small functions,2 or 3 lines
  - Frequently called functions, e.g. inside loops
- Not inline?
  - Very large functions, more than 20 lines
  - Recursive functions



### 18 Constant Objects

- What if an object is const?

  ```C++   
   const Currency the_raise(42，38);
  ```

- What members can access the internals?
- How can the object be protected from change?

#### 18.1 Const member functions

```c++
int Date::set day(int d){
    //...error check d here...
    // ok, non-const so can modify 
    day=d;
}
int Date::get day() const {
	day++;  //ERROR modifies data  member

	set_day(12); // ERROR calls non-const  member

	return day;  // ok /64
}
```

#### 18.2 Const member function usage

- Repeat the const keyword in the definition as well as the declaration
  int get day () const;
  int get day() const { return day };
- Function members that do not modify data should be declared const
- const member functions are safe for const  objects

const 实际是 给 this这个指针的

```c++
#include <iostream>
using namespace std;
class Date{
    int day;
    public:
        Date():day(1){};
        void set_day(int d) const;
        int get_day() const;
		//以下 f()可以重载吗 overload?为什么？ 
        void f(){cout << " f() "<< endl;};  //实际上是 f(Date * this){....}
        void f()const{cout << " f() const"<< endl;} //实际上是 f(const Date *this){...}
};
void Date::set_day(int d) const{
   // day=d;  //Error
}

int Date::get_day() const{
  //  day++;  //error
    set_day(12);
    return day;
}

int main(){
    Date d;
    d.f();  //打印输出是什么？f() 
    const Date b;
    b.f(); //打印输出是什么？f() const
    return 0;
}
```



#### 18.5 Constant Data menber in class

```c++
class A{
	const int i;
    public:
        A():i(0){//i 只能通过初始化列表初始化

        }
}
int main(){
    A a;  //Error : error: uninitialized const member in 'class A'; 'const int A::i' should be initialized
    A a(1);  //    error: assignment of read-only member 'A::i'

}
```

- Has to be initialized in **initializer list** of the  constructor

#### 18.6 Compile-time constants in classes

```c++
class HasArray {
	const int size;
	int array[sizel; // ERROR!
          ...
  	};
          
```

- Make the const value static:
  ```c++
  static const int size = 100;
  ```

  **static** indicates **only one per class** (not one per object)

- Or use anonymous enum"hack

  ```c++
  class HasArray{
     
      enum  { size = 100 };
      int array[size]; // OK!
      ...
  }
  ```

  



### 21 Declaring references 引用

- References are a new data type in C++

  ```c++
  - char c; 			//a character 
  - char* p = &c; 	// a pointer to a character
  - char& r = c;	 	// a reference to a character
  ```

  

- Local or global variables

​		 - type& refname = name

​		- For ordinary variables, the initial value is required

  -  In parameter lists and member variables

​	- type& refname

​	- Binding defined by caller or constructor

#### 21.2 References
- Declares a new name for an existing object

```c++
int  X = 47;
int& Y = X; // Y is a reference to X
// X and Y now refer to the same variable
cout <<" Y= "  << y; // prints Y = 47
Y=18;
cout <<" X= "  << x;  // prints X = 18
```

#### 21.3 Rules of references

- References must be initialized when defined

- Initialization establishes a binding

  ```c++
  - In declaration
  int x = 3;
  int& y = x;
  const int& z = x;  //不能通过修改z 修改x; 但可以修改 x, 则z也改变了
  - As a function argument
  void f( int& x );
  f(y); // initialized when function is called
  ```

- Bindings don't change at run time, unlike pointers

  ```c++
  int& y = x;
  y = 12; // Changes value of x
  ```

- Assignment changes the object referred-to

```C++
void func(int &);
func (i * 3);  // Warning or error!
```



<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107221332254.png" alt="image-20241107221332254" style="zoom:67%;" />

#### 21.5 Rules of references

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241107221704566.png" alt="image-20241107221704566" style="zoom:67%;" />

#### 21.6 Restrictions

- No references to references

- No pointers to references
  ```c++
  int&* p;  //illegal
  ```

- Reference to pointer is ok
  void f(int*& p);
- No arrays of references



<<<<<<< HEAD
### 多态性

![image-20241125194123919](D:\OfficeSpace\MarkdownNotes\assets\image-20241125194123919.png)



![](D:\OfficeSpace\MarkdownNotes\assets\image-20241125194451865.png)



```C++
Shape
Define the general properties of a Shape
class XYPos{...}; // x,Y point
class Shape {
public:
    Shape ();
    virtual ~Shape();
    virtual void render();
    void move(const XYPos&);
    virtual void resize();
protected:
	XYPos center;
};
```

#### 23.2 Add new shapes

~~~c++
class Ellipse : public Shape {
    public:
    	Ellipse(float maj, float minr);
    	virtual void render(); // will define own
    protected:
    	float major_axis, minor_axis;
}
class Circle : public Ellipse {
    public:
    	Circle(float radius) : Ellipse(radius, radius){}
    	virtual void render();
}
~~~



#### 23.3 Sample

```c++

void render(Shape *p) {
	p->render(); // calls correct render function for given Shape!
}
void func() {
    
    Ellipse ell(10，20);
    ell.render();
    
    Circle circ(40);
    circ.render();
    
    render(&ell);
    
    render(&circ);
}
```



### 23.4 Polymorphism 多态性



 以上例子什么是多态的？这里 P是多态的， shape中 定义 p->render() 时，是不知道 后续会有什么具体形状的子类的，只有再执行时才知道是，Ellipse 的render(), retangle的render(),  circle的render() . 所以，p是多态的。

- **Upcast**: take an obiect of the derived class as an object of the base one.

  - Ellipse can be treated as a Shape

- **Dynamic binding**

  - Binding: which function to be called

    - Static binding: call the function as the code  编译时就绑定哪个函数， move()是没有virtaul, 是静态绑定

    - Dynamic binding:  call the function of the object 

      virtual 的是 地址指向具体对象时，动态绑定函数

#### 24.4 How virtual works in C++



<img src="D:\OfficeSpace\MarkdownNotes\assets\企业微信截图_17325377401835.png" alt="企业微信截图_17325377401835" style="zoom: 67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125205312239.png" alt="image-20241125205312239" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125221143320.png" alt="image-20241125221143320" style="zoom:67%;" />







<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241125221325540.png" alt="image-20241125221325540" style="zoom:67%;" />

#### 24.5 what happens if



```C++
Ellipse elly(20F， 40F);
Circle circ(60F);
elly= circ; //10 in 5?
```

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125223134706.png" alt="image-20241125223134706" style="zoom:67%;" />





<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241125223312058.png" alt="image-20241125223312058" style="zoom:67%;" />



<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125223427978.png" alt="image-20241125223427978" style="zoom:50%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125224251616.png" alt="image-20241125224251616" style="zoom:50%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125230032430.png" alt="image-20241125230032430" style="zoom: 67%;" />

![image-20241125231636073](D:\OfficeSpace\MarkdownNotes\assets\image-20241125231636073.png)

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241125231758613.png" alt="image-20241125231758613" style="zoom:67%;" />

![image-20241125231954241](D:\OfficeSpace\MarkdownNotes\assets\image-20241125231954241.png)

#### 24.6 References as class members

- Declared without initial value
- Must be initialized using constructor initializer list

```c++
class X {
public:
	int& m_y;
	X(int& a);
	X::X(int& a) : m_y(a) { }; //必须使用初始化列表
};
```

#### 24.7 Returning references

- Functions can return references
  -But they better refer to **non-local** variables!

  ```c++
  #include <assert.h>
  const int SIZE = 32;
  double myarray[SIZE];
  double& subscript(const int i) {
      return myarray[il;  //与指针类似， 但不能是函数的本地变量的reference
    }
  
     int main(){
         for(int i=0;i<SIZE;i++){
             myarray[i] = i * 0.5;
         }
         double value = subsripts(12);
         subsripts(3) =34.5;   // 34.5 赋值给返回的哪个变量
     }
                     
                     
  ```
  

#### 24.5 const in Functions Arguments

- Pass by const value -- don't do it

- Passing by const reference
Person( **const string&** name, int weight );

- don't change the string object

- more efficient to pass by **reference** (**address**) than to pass by **value** (copy)

- const qualifier protects from change 

  所以，在c++中传对象参数 时使用  引用，这样不会复制对象 提高效率。

  若不想被修改，使用 const &作为参数



#### 24.6 Temporary values are const

- What you type

```c++

void func(int &);
func (i * 3); // Generates warning or error !
```

- What the compiler generates

```C++
void func(int &);
const int tmp@ = i * 3;
func(tmp@); // Problem -- binding const ref to non-const argument!
```

The temporary is constant, since you can't access it

### 26. Copy 拷贝构造

#### 26.1 Copying

- Create a new object from an existing one
  -For example, when calling a function

  ```c++
  // Currency as pass-by-value argument
  void func(Currency p) {
  cout <<"x="<< p.dollars();
  }
  ...
  Currency bucks(100， 0);
  func(bucks); // bucks is copied into p
  ```

  

初始化 和赋值在c中一样，但在 c++完全不一样。



Current p =bucks;  //初始化

p = bucks;//赋值



![企业微信截图_1732630232608](D:\OfficeSpace\MarkdownNotes\assets\企业微信截图_1732630232608.png)

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241126221552253.png" alt="image-20241126221552253" style="zoom:67%;" />



![image-20241126222908494](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241126222908494.png)

 h2=f(h);

h2=h; 没有经过构造函数，但经过析构函数

![image-20241126223832111](D:\OfficeSpace\MarkdownNotes\assets\image-20241126223832111.png)
=======













>>>>>>> 2d306f190c2d4a59a28fb92f4e52e57ecfa47cdd



## Reference: 

- [C++ More Basics](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/cp2_MoreBasics.html)
<<<<<<< HEAD



## C语言补充

### 1.函数原型

#### 1.1 函数的先后关系

![image-20241127214201422](D:\OfficeSpace\MarkdownNotes\assets\image-20241127214201422.png)

![image-20241127215932139](D:\OfficeSpace\MarkdownNotes\assets\image-20241127215932139.png)

![image-20241127220055725](D:\OfficeSpace\MarkdownNotes\assets\image-20241127220055725.png)



![image-20241127220641430](D:\OfficeSpace\MarkdownNotes\assets\image-20241127221410889.png)

类型不匹配?

调用函数时给的值与参数的类型不匹配是C语言传统上最大的洞
编译器总是悄悄替你把类型转换好，但是这很可能不是你所期望的后续的语言，C++/Java在这方面很严格

#### 运算符 &

获取变量的地址。它的操作数是变量。

```c
int i = 0; 
printf("0x%x\n", &i);
printf("%p\n",&i);//输出地址
```

#### scanf
- 如果能够将取得的变量的地址传递给一个函数
- 能否通过这个地址在那个函数内访问这个变量?
  scanf(“%d”,&i)
- scanf()的原型应该是怎样的? 我们需要一个参数能保存别的变量的地址，如何表达能够保存地址

#### 访问那个地址上的变量

*是一个单目运算符，用来访问指针的值所表示的地址上的变量
。可以做右值也可以做左值
·int k=*p;
。*p=k+1;

#### 指针应用场景一
- 交换两个变量的值

  ```c
  void swap(int *pa,int *pb){
  int t = *pa;
  *pa = *pb;
  *pb = t;
  }
  ```

#### 指针应用场景二
- 函数返回**多个值**，某些值就只能通过指针返回
- 传入的参数实际上是需要保存带回的结果的变量

```C
#include <stdio.h>
void minmax(int a[], int len, int *max, int *min);
int main(void){
    int a[] = {1,2,3,4,5, 6,7,8,9,12,13,14,16,17,21, 23, 55,};
    int min,max;
    minmax(a，sizeof(a)/sizeof(a[0])，&min，&max);
    printf("min=%d,max=%d\n"，min，max);
    return 0;
  }
//找到最大值 和最小值
void minmax(int a[],int len,int *min,int *max){
    int i;
    *min = *max=a[0];
    for ( i=l; i<len; i++ ){
        if (a[i] <*min ){
         *min = a[i];
        }
        if ( a[i] >*max){
            *max = a[i];
        }
      }
 }
```



#### 指针应用场景二b

函数返回运算的状态，结果通过指针返回
常用的套路是让函数返回特殊的不属于有效范围内的值来表示出错

- ·-1或0(在文件操作会看到大量的例子)
- 但是当任何数值都是有效的可能结果时，就得分开返回了

```c
#include <stdio.n>
/*
@return 如果除法成功，返回1;否则,返回0
*/
int divide(int a,int b, int *result);
int main(void){
    int a=5;
    int b=2;
    int c;
    if ( divide(a,b,&c) ) {
    	printf("%d/%d=%d\n"，a，b，c);
    return 0;
    }
int divide(int a, int b,int *result){
    int ret = 1;
    if ( b == @ ) 
        ret = 0;
    else {
    	*result = a/b;
    }
    return ret;
}

```

#### 指针最常见的错误

- 定义了指针变量，还没有指向任何变量，就开始使用指针

#### 传入函数的数组成了什么?

```c
#include <stdio.h>
void minmax(int a[], int len, int *max, int *min);
int main(void){
    int a[] = {1,2,3,4,5, 6,7,8,9,12,13,14,16,17,21, 23, 55,};
    int min,max;
    minmax(a，sizeof(a)/sizeof(a[0])，&min，&max);
    printf("min=%d,max=%d\n"，min，max);
    return 0;
  }
//找到最大值 和最小值
void minmax(int a[],int len,int *min,int *max){
    int i;
    *min = *max=a[0];
    for ( i=l; i<len; i++ ){
        if (a[i] <*min ){
         *min = a[i];
        }
        if ( a[i] >*max){
            *max = a[i];
        }
      }
 }
```

#### 数组参数

```c
以下四种函数原型是等价的:
int sum(int *ar, int n);
int sum(int*, int);
int sum(int ar[], int n):
int sum(int [], int);
```

#### 数组变量是特殊的指针

```C
//数组变量本身表达地址，所以
int a[10];int*p=a: // 无需用&取地址
//但是数组的单元表达的是变量，需要用&取地址
 a==&a[0]
// []运算符可以对数组做，也可以对指针做
p[0] <==> a[0]
```



### 26.Static in C++

Two basic meanings:

- Static storage
  -allocated once at a fixed address
- Visibility of a name
  -internal linkage
- Don't use static except inside functions and classes.

#### 26.1 Uses of“static”in C++

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241202221251298.png" alt="image-20241202221251298" style="zoom:67%;" />

### 30 运算符重载 Overloading Operators

- Allows user-defined types to act like built in types
- Another way to make a function call
- Unary and binary operators can be overloaded
   ![企业微信截图_17331489175847](D:\OfficeSpace\MarkdownNotes\assets\企业微信截图_17331489175847.png)

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241202221722551.png" alt="image-20241202221722551" style="zoom:67%;" />

#### 30.1 Restrictions

- Only existing operators can be overloaded (you can't create a ** operator for exponentiation)
- Operators must be overloaded on a class or enumeration type
- Overloaded operators must -Preserve number of operands
  -Preserve precedence

#### 30.2 C++ overloaded operator

- Just a function with an operator name!
  -Use the **operator** keyword as a prefix to name 

  **operator** *(...)

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241202222354828.png" alt="image-20241202222354828" style="zoom:67%;" />

  #### 30.3 How to overload

- As member function
  -Implicit first argument
  -No type conversion performed on receiver
  -Must have access to class definition

  #### 30.4 Operators as member functions

  ```c++
  class Integer {
  	public:
      Integer( int n = 0 ) : i(n) {}
      const Integer operator+(const Integer& n) const{
      return Integer(i + n.i);
          ...
      private:
      int i;
  }
  ```

  #### 30.5 Member Functions

  ```c++
  Integer x(1)，y(5)，z;
  x + y;   // => x.operator+(y) ;
  ```

- lmplicit first argument

- Developer must have access to class definition

- Members have full access to all data in class

- No type conversion performed on receiver

  ```c++
  Z=X+y   //0k X是receiver
  Z=X+3	//ok X是receiver
  z=3 +y	// err 编译通不过   3是receiver
  Z=X+3.5	//err
  ```

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241202224335944.png" alt="image-20241202224335944" style="zoom:67%;" />

  #### 30.5 Operator as a global function

  ```c++
  const Integer operator+(
  const Integer& rhs,
  const Integer& lhs);
  Integer x， y;
  x + y  // ====> operator+(x，y);
  ```

- Explicit first argument

- Developer does not need special access to classes

- May need to be a friend

- Type conversions performed on both arguments

  #### 30.6 Global Operators

  - binary operators require two arguments

  - unary operators require one argument
    conversion:

    ```c++
    z= x + y;
    Z=x+3;
    z=3+y;
    z=3+7;
    ```

    lf you don't have access to private data members, then the global function must use
    the public interface (e.g. accessors)

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241202225042429.png" alt="image-20241202225042429" style="zoom:67%;" />



#### 30.7 Argument Passing

- if it is read-only pass it in as a const reference except built-ins)
- make member functions const that don't change the class (boolean operators, +, -, etc)
- for global functions, if the left-hand side changes pass as a reference (assignment operators)

#### 30.8 Return values

- Select the return type depending on theexpected meaning of the operator. For
  example
  -For operator+ you need to generate a new object. Return as a const object so the result cannot be modified as an lvalue.
  -Logical operators should return bool (or int for older
  compilers)

  #### 30.9 The prototypies of operators

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203125400009.png" alt="image-20241203125400009" style="zoom:67%;" />

  <img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203125546232.png" alt="image-20241203125546232" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203125728329.png" alt="image-20241203125728329" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203125857082.png" alt="image-20241203125857082" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203125931057.png" alt="image-20241203125931057" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203130049286.png" alt="image-20241203130049286" style="zoom:67%;" />

<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203130219312.png" alt="image-20241203130219312" style="zoom:67%;" />



<img src="D:\OfficeSpace\MarkdownNotes\assets\image-20241203130315327.png" alt="image-20241203130315327" style="zoom:67%;" />

#### 30.12 Value classes

- Appear to be primitive data types
- Passed to and returned from functions
- ·Have overloaded operators (often)
- Can be converted to and from other types
- ·Examples: Complex, Date, String



[第08课【 C++运算符重载】运算符重载，特殊重载，operator隐式转换，[\]和()重载_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19u4y117Ge/?spm_id_from=333.337.search-card.all.click&vd_source=265631a940f0c085c9ceec6d0254cd21)

=======
- 
>>>>>>> 2d306f190c2d4a59a28fb92f4e52e57ecfa47cdd
