## 3.2 线性表

### 3.2.1 线性表定义

线性表(Line List)是由(       )的数据元素构成的(       )的线性结构。

线性表中元素的(     )称为线性表的长度。

假设定义线性表 :

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



