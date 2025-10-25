## 3.2 线性表

### 3.2.1 线性表定义

线性表(Line List)是由(       )的数据元素构成的(       )的线性结构。

线性表中元素的(     )称为线性表的长度。

<<<<<<< HEAD
####  操作集

- List MakeEmpty( )初始化一个空列表
- Position Find(List L, ElementType x) 查到X相同的值，找到返回 X的位置， 否则，返回 -1
- bool Insert(List L, ElementType x, Position i) 在 指定序位 i插入 X
- int length(List L)  返回L长度

#### 定义线性表 :
=======
假设定义线性表 :
>>>>>>> 2d306f190c2d4a59a28fb92f4e52e57ecfa47cdd

```c
#define MAXSIZE 100   //线性表最大存储空间， 即， 最多存放 100个元素
#define ERROR -1

typedef int ElementType; 	//元素类型

struct list
{
    ElementType data[MAXSIZE];
    int last;	//最后一个元素的 索引(从0开始)
};

typedef struct list *List;
```
<<<<<<< HEAD
#### 查找

```c
// 2.查找 
int find(ElementType x)
{
    int i = 0;
    List list = createList();
    while (i < list->last && list->data[i] != x)
    {
        i++;
    }
    if (i > list->last)
        return ERROR;
    else
        return i;
}
```

1. 顺序表中做插入平均移动表中一半的元素，插入的平均时间复杂度为 :  O(n)

2) 顺序表中有（         ）存储单元，在表满时不能再插入，否则产生（       ） 错误。

3) i 指元素的序号 非 数组中的下标，i 有效范围是(                                 )

#### 删除

1. 将表中位序为 i 的元素从线性表中去掉。i 的取值范围(   1<= i <= MAXSIZE   )

2. 删除步骤：

   (1)将a[i] - a[n] 顺序向前移动,  a[i] 元素被aa[i+1]覆溢;
   (2)修改 Last 指针(相当于修改表长)使之仍指向最后一个元素;

```c
bool delete(List list, int i){

    if( i<1 || ________________________){
        printf("位序%d不合法\n",i);
        return false;
    }
    //移动元素
    for(int j= ___; j< _______;j++)
        list->data[j] =list->data[j+1];
    
    //修改Last指针
    list->last --;
    
    return true;
}
```

本函数中注意以下问题:
(1)删除位序为i的元素, i 的取值必须为(                  ),否则该元素不存在。 
(2)当表空时不能做删除, 表空时 L->Last 的值为(                )  
(3)删除 平均移动次数则为 n/2,  这说明顺序表上作删除运算时平均需要移动表中一半的元素, 显然该算法的时间复杂度为  O(n)。



**求表长**： 

```c
 int length(List list){
     
     return list->last+1;
 }
```





### 3.2.3 线性表的链式存储实现

- 单向链表: 每个数据单元由**数据域**和**链接域**两部分组成。（          ）用来存放数值。（               )是线性表数据单元的结构指针。

- 访问链表,必须先找到链表的第一个数据单元, 因此, 实际应用中常用一个称为(Header)的指针指向链表的第一个单元,并用它表示一个具体的链表。

  

  1). 带**头指针**的链表：

![image-20241114205149459](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241114205149459.png)

2). 带**头结点**的链表：

![image-20241114205044103](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241114205044103.png)

#### 3.2.3.1 结构定义

~~~c
typedef int ElementType;

struct list_node
{
    ElementType data;
    struct list_node *next;;
};

typedef struct list_node *List;
~~~



####  3.2.3.2求表长

时间复杂度 O(n)

```c
int length(List list){
    List p;
    int cnt=0;  /*初始化计数器*/
    p =list;    /*p指向表的第一个结点。*/
    while(p){
        p=p->next;
        cnt++;
    }  
    return cnt;
}
```

#### 3.2.3.3 查找

线性表的查找有两种,即按序号查找(FindKth)和按值查找(Find)两种。
(1)按序号查找 FindKth（参数是序号）,  返回 元素的值
 (2)按值查找，即定位 Find，参数是 元素值，返回中结点的指针

```c
```



#### 3.2.3.4 插入

线性表的插人是在指定位序i(             )前插人一个新元素 X。

- 当插人位序为 1 时代表插人到链表的头;
- 为n+1时,代表插人到链表最后。
- 基本思路是:  
  - 如果i不为 1,则找到位序为  i 的结点 pre; 
    - 若存在,则申请一个新结点并在数据域填上相应值， 然后将新结点插人到结点 pre之（    ）,返回结果链表;
    - 如果不存在则返回错误信息。

```c
```

注意:在上述函数中表头指针L的值可能会发生变化:

- 当插人发生在**表头结点**时, 头指针向新的表头结点(可利用函数返回值对 list 重新赋值);

- 其他情况下list 值不变。所以，在本函数中 list 既作为函数参数, 同时也作为函数返回值,保证新的list值能够被带回来。

- 但是因为函数操作不成功时,返回的指针为NULL,所以，我们又不能直接用 “list =Insert(list,X,i)"来调用的。 
  用一个临时指针接收插人两数的返回值,  根据该指针的值判断是否应该更新list;   这样与线性列表插入函数不一致

- 一种解决的方法是： 为链表增加一个空的“头结点”,真正的元素链接在这个空结后。这样做的好处是,无论在哪里插人或者删除, list的值一直指向固定的空结点,不会改变

- 带头结点的链式表的插人函数, 插人算法的时间复杂度为 O(n)。

  ![image-20241114205044103](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20241114205044103.png)

```c
//插入1
List insertA(List list, ElementType x, int i)
{
    List tmp=(List)malloc(sizeof(struct list_node));
    tmp->data =x;

    if(i==1){
        tmp->next=list;
        return tmp;
    }

    List pre=list;
    int cnt=0;
    while(pre&&cnt<i-1){
        pre=pre->next;
        cnt++;
    }
     
    if (pre==NULL || cnt!=i-1)
    {  
        printf("位序%d不合法\n",i);
        free(tmp);
        return ERROR;
    }else{
        tmp->next = pre->next;
        pre->next = tmp;
    }
    return list;
}
```

```c
//带头结点链表插入
bool insert(List list, ElementType x, int i)
{
    //假设链表带头结点
    List pre=list;
    int cnt=0;
    while(pre&&cnt<i-1){
        pre=pre->next;
        cnt++;
    }
     
    if (pre==NULL || cnt!=i-1)
    {  
        printf("插入位序%d不合法\n",i);
        return false;
    }else{
        List tmp=(List)malloc(sizeof(struct list_node));
        tmp->data =x;
        tmp->next = pre->next;
        pre->next = tmp;
    }
    return true;
}
```



#### 3.2.3.5 删除

在单向链表中删除指定**位序i**的元素,首先需要找到被删除结点的前一个元素, 然后, 再**删除结点**并**释放空间**。代码 3.10是**带头结点的链式表**的删除。时间复杂度为 0(n) 

```c
bool Delete(List list, int i){

    List pre=list;
    int cnt=0;

    while(pre&& cnt<i-1){
        pre = pre->next;
        cnt++;
    }
    if (pre==NULL || cnt!=i-1)//所找结点或位序为的结点不在List中
    {  
        printf("删除位序%d不合法\n",i);
        return false;
    }else{/*找到了待删结点的前一个结点 pre*/
        List tmp=pre->next;
        pre->next= tmp->next;
        free(tmp);
    }
    return true;
}
```



### 3.3 堆栈

**堆栈(Stack)**可以认为是具有一定约束的线性表,  插入和删除操作都作用在一个称为**栈顶(Top)**的端点位置。

对该序列的主要操作是在序列的（      ）插人元素和删除(取出)元素。有这类操作要求的序列我们称之为“堆栈”

- 数据插人称为**压人栈**(Push),而数据删除可看作从堆栈中取出数据,叫做**弹出栈**(Pop)。
- 堆栈也被称为**后入先出**(Last In First Out,LIFO)表。

#### 3.3.1 堆栈的抽象数据类型定义为:

类型名称:		堆栈(Stak)。
数据对象集:	一个有0个或多个元素的有穷线性表。
操作集:		对于一个具体的**长度**为正整数 MaxSize 的堆Stack, 

堆栈的基本操作主要有:
(1) **Stack CreateStack(int MaxSize)** :	生成空堆栈,其最大长度为 MaxSize。

入参是(                        ).     返回是(                                       )

(2)**bool IsFull(Stack s)**:判断堆S是否已满。若S中元素(      )等于（     ）  时返回 **true**; 否则返回**false**;
(3)**bool Push(Stack S, ElementType X)**:  将元素X压入堆 。若堆 已满,  返回false;否则，将数据元素X插入到堆栈S（      ） 处并返回 true;

入参是(                        ).     返回是(                                       )

(4)**boolIsEmpty(Stack S)**;   判断堆 S是否为空,若是返回 true;   否则返回false。
(5)**ElementType Pop(Stack S)**:   删除并返回顶元素。若堆 为空,返回错误信息;否则将（     ）数据元素从堆栈中删除并返回。

入参是(                        ).     返回是(                                       )

#### 3.3.2 堆栈实现



### 二叉树

#### 4.0 树

**高度**、**深度**和**平衡因子**

（1）深度——从上往下数

**节点的层次（节点的深度）：**从根开始定义，根为第1层，根的子节点为第2层，以此类推； 也可以说成根节点的深度为1。

**树的深度：**树中节点的最大层次

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190811094415965-1541024368.jpg)

（树的深度 = 叶子节点的深度）

2）高度——从下往上数

关于高度， 空二叉树的高度为0， 叶子节点的高度为1

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190812105349161-137592344.jpg)

（树的高度 = 根节点的高度）

（3）平衡因子

某结点的左子树与右子树的高度或深度(高度深度都可以，本篇随笔使用深度来计算平衡因子)差即为该结点的平衡因子（BF,Balance Factor），平衡二叉树（AVL树）上所有结点的平衡因子只可能是 -1，0 或 1

从上面的节点的定义可以看出，节点中存储的是节点的高度，而不是平衡因子

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190811100452547-325362680.jpg)

 

下图中就标注了所有节点的平衡因子

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190812105620385-690446851.jpg)

 （平衡因子计算时左子树 - 右子树 和 右子树 - 左子树 都可以，因为判断树是否平衡的条件是：每个结点的左右子树的**高度之差的绝对值**不超过1，只不过判断失衡以后还要判断是哪一种失衡，这就需要根据情况来选择是左-右还是右-左了）



#### 4.3 完全二叉树- Complete-Binary-Tree

- We know a ***complete binary tree** is a tree in which except for the last level (say **L**)all the other level has (**2L**) nodes and the nodes are lined up **from left to right side**.
- It can be represented using an array. If the parent is it index **i** so the left child is at **2i+1** and the right child is at **2i+2**.

##### 4.3.1 完全二叉树 顺序存储结构：

普通的二叉树是不适合用数组来存储的，因为**可能会存在大量的空间浪费**。而**完全二叉树更适合使用顺序结构存储**。现实中我们通常把堆(一种二叉树)使用顺序结构的数组来存储。

需要注意的是这里的堆和操作系统虚拟进程地址空间中的堆是两回事，一个是数据结构，一个是操作系统中管理内存的一块区域分段。



![img](D:\OfficeSpace\MarkdownNotes\assets\ih6f775xe5bui_7fa516b3c93d4d07b05ea99d47a61e21.png)

[【堆】数据结构堆的实现（万字详解）-阿里云开发者社区](https://developer.aliyun.com/article/1460853) 

N个结点的完全二叉树中，对于下标 i 的结点，**注意，数组起始单元的下标 1，不是 0**

-  父结点 ：     **⌈   i/2 ⌉**  

-  左孩子:         **2i**                    2i   <= N

- 右孩子:           **2i+1**               2i+1 <= N

  ![Lightbox](D:\OfficeSpace\MarkdownNotes\assets\full-300x140.jpg)

![img](D:\OfficeSpace\MarkdownNotes\assets\arr-300x77.jpg)

![Lightbox](D:\OfficeSpace\MarkdownNotes\assets\com-300x165.jpg)![Lightbox](D:\OfficeSpace\MarkdownNotes\assets\arr1-300x116.jpg)



一般二叉树：

![Lightbox](D:\OfficeSpace\MarkdownNotes\assets\cnf-300x149.jpg)![Lightbox](D:\OfficeSpace\MarkdownNotes\assets\arr2-300x93.jpg)

#### 4.3.2 链表存储









#### 4.3.3 二叉树的创建

2.二叉树的创建
由于树是非线性结构,创建一棵二叉树必须首先**确定树中结点的输人顺序**, 常有的方法是先序创建和层序创建两种。
层序创建所用的结点输入序列是按树的从上至下从左到右的顺序形成的,各层的空结点转入人数值0。在构造二叉树过程中,需要一个队列暂时存储各结点地址,其创建过程如下。
(1)输人第一个数据:
·若为0,表示是此树为空,将空指针赋给根指针,树构造完毕;
·若不为0,动态分配一个结点单元,并存人数据,同时将该结点地址放入队列。
(2) 若队列不为空,从队列中取出一个结点地址,并建立该结点的左右孩子:



### 4.4 搜索二叉树 Binary- Search-Tree   BST



### 4.5平衡二叉树  Balanced-Binary-Tree    AVL

#### 4.5.1 平均搜索长度： 

- ASL 值越小，结构越好，越接近完全二叉树。查找时间复杂度越接近 O(Log n).

- AVL 树查找、插入、删除 时间复杂度越接近 O(Log n)。

- AVL 树的任何一个结点 都是 AVL树

- 根结点左右高度差的绝对值不超过 1

- 对于二叉树中的任一个结点T， 平衡因子(Balance Factor, BF)= H_left  - H_Right

- AVL 树 BF = { -1, 0, 1}

- AVL 树结点 除了一般二叉树的数据成员外，还需要增加 平稳信息。 假设增加结点的高度,定义整型 height.

- 增加 getHeight(  T ) 函数获取 树 T的高度

  [AVL树（查找、插入、删除）——C语言 - Luv3 - 博客园](https://www.cnblogs.com/lanhaicode/p/11321243.html)

  

  #### 4.5.2 旋转

- right-rotate

  ![image-20241122113011417](D:\OfficeSpace\MarkdownNotes\assets\image-20241122113011417.png)

  ​		

  ```c
  
  ```

  

- left

  ![image-20241122113036434](D:\OfficeSpace\MarkdownNotes\assets\image-20241122113036434.png)

[C Program to Implement AVL Tree - GeeksforGeeks](https://www.geeksforgeeks.org/c-program-to-implement-avl-tree/)

[AVL树（查找、插入、删除）——C语言 - Luv3 - 博客园](https://www.cnblogs.com/lanhaicode/p/11321243.html) 

#### 4.5.3  **删除节点**

删除节点比插入节点的操作还要稍微复杂一点，因为插入时，进行一次平衡处理（一次平衡处理可能包含多次旋转），整棵树都会处于平衡状态，而在删除时，需要进行多次平衡处理，才能保证树处于平衡状态

AVL树的删除操作前半部分和二叉查找树相同，只不过删除后要检查树是否失去平衡，如果失衡就需要重新调整平衡，并更新节点高度，总的来说可以分为如下几种情况

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190813200748316-1794055915.jpg)

**（1）删除叶子节点**

情况一：删除节点后二叉树没有失去平衡

<img src="D:\OfficeSpace\MarkdownNotes\assets\1590962-20190813174200519-739313971.png" alt="img" style="zoom:67%;" />

 

删除节点后树没有失去平衡，这种情况下只需要**更新节点的高度**

情况二：删除节点后二叉树失去平衡

![img](D:\OfficeSpace\MarkdownNotes\assets\1590962-20190813212427443-2023063416.png)

上图的**RE型**失衡只有在删除操作时才可能出现（在插入时不可能出现），**RE型失衡**的旋转方式和**RR型失衡**的旋转方式一模一样

（虽然删除节点时遇到的失衡情况多了两种 LE和RE ，但是旋转的方式依旧是那四种（LL、RR、LR、RL））



### 4.6 最小堆 Heap    也叫优先队列  Priority Queue

- 堆是特殊队列，按元素大小优先从堆中取出元素， 而不是元素进入堆的时间顺序(Queue是按时间顺序)

- 堆 是完全二叉树

- ***\*Min Heap:\**** Each node is less than or equal to the value of its child subtrees.

- ***\*Max Heap:\**** Each node is greater than or equal to the value of its child subtrees.

- Heap一般用数组方式实现，数组起始元素位置有两种方式

  -  从i = 1 开始

    - Left child:  **2 \* i **

    - Right child:  **2 \* i + 1** 

    - Parent:   **⌊ i/2⌋**     向下取整 

       

      |             | i          | 1    | 2    | 3    | 4    |
      | ----------- | ---------- | ---- | ---- | ---- | ---- |
      | parent      | **⌊ i/2⌋** | 0    | 1    | 1    | 2    |
      | left child  | 2 \* i     |      |      |      |      |
      | right child | 2 \* i +1  |      |      |      |      |

  - 从i = 0 开始

    - Left child:  **2 \* i + 1**

    - Right child:  **2 \* i + 2** 

    - Parent:  **(i - 1) / 2**

      |             | i           | 0    | 1    | 2    | 3    | 4    |
      | ----------- | ----------- | ---- | ---- | ---- | ---- | ---- |
      | parent      | **(i-1)/2** | 0    | 0    | 0    | 1    | 2    |
      | left child  | 2 \* i +1   | 1    | 3    | 5    |      |      |
      | right child | 2 \* i +2   | 2    | 4    | 6    |      |      |
=======


>>>>>>> 2d306f190c2d4a59a28fb92f4e52e57ecfa47cdd

