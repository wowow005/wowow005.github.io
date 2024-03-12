# 用 C 语言实现常见数据结构


# 用 C 语言实现常见数据结构

## 线性表

线性表是 $n$ 个数据元素组成的有序序列。

### 顺序表

线性表的顺序存储即为顺序表。

我们用下面的结构类型来 **定义** 顺序表：

```c
#include <stdio.h>
#define ListSize 100

#define ERROR 0
#define OK 1
typedef int Status;

// 定义顺序表
typedef int DataType;
typedef struct
{
  DataType data[ListSize];
  int length;
} SeqList;
```

可以看到，我们将数据元素放入一个一维数组中，再用一个 `int` 变量 `length` 来表示表长。

接着，我们来构造一个函数用来 **初始化** 这个顺序表：

```c
// 初始化顺序表
Status InitList (SeqList *list)
{
  list->length = 0;
  return OK;
}
```

这样，一个基本的顺序表就可以使用了。

接下来，我们可以实现一些顺序表的运算，首先是插入，插入运算需要 3 个参数，
插入的表名，插入的位置，插入的值。

**插入函数** 如下所示：

```c
// 在指定位置插入元素
Status InsertList(SeqList *list, int position, DataType value) {
  if (list->length >= ListSize || position < 0 || position > list->length + 1) {
    printf("无法插入元素\n");
    return ERROR;
  }

  for (int i = list->length; i >= position; i--) {
    list->data[i] = list->data[i-1];
  }
  list->data[position - 1] = value;
  list->length++;

  return OK;
}
```

所以我们在后面创建一个顺序表的时候，就要用到插入函数。

然后是 **删除函数**，删除函数需要两个参数，如下所示：

```c
// 在指定位置删除元素
DataType DeleteList(SeqList *list, int position) {
  int i;
  DataType value;
  if (position < 1 || position > list->length) {
    printf("position error");
    return ERROR;
  }

  value = list->data[position - 1];
  for (i = position; i < list->length; i++)
    list->data[i-1] = list->data[i];
  list->length--;
  return value;
}
```

我们还要构造一个 **打印顺序表** 函数，用于打印整个顺序表：

```c
// 打印顺序表
void PrintList(SeqList *L) {
  for (int i = 0; i < L->length; i++) {
    printf("%d ", L->data[i]);
  }
  printf("\n");
}
```

最后就是 **主函数** 了，我们写一些简单的测试：

```c
int main() {           
  SeqList mylist;
  InitList(&mylist);

  InsertList(&mylist, 1, 10);
  InsertList(&mylist, 2, 20);
  InsertList(&mylist, 3, 30);

  printf("After inserting 10, 20, 30:\n");
  PrintList(&mylist);

  DeleteList(&mylist, 2);

  printf("after deleteing element at position 2:\n");
  PrintList(&mylist);

  return 0;
}
```

我们可以看到终端的输出为：

```c
After inserting 10, 20, 30:
10 20 30 
after deleteing element at position 2:
10 30 
```

## 链表

链式存储的线性表我们称为链表，又可以分为单链表、循环链表和双向链表等。

我们一般用结点来表示链表的数据元素，它由数据域和指针域组成。

### 单链表

由 $n$ 个结点组成的链表，并且结点只包含一个指针域，这样的链表称为单链表。

![](https://upload.wikimedia.org/wikipedia/commons/4/45/Link_zh.png)

C 语言中 **单链表的定义** 如下：

```c
#include <stdio.h>

// 单链表结点的定义
typedef struct node {
  DataType data;
  struct node * next;
} ListNode;

// 定义指向结点的指针变量别名
typedef ListNode * LinkList;
```

**单链表的建立** 有两种，可以用头插法建表或者用尾插法建表。

头插法建表：

```c
// 头插法建立单链表
LinkList CreateListF() {
    LinkList head = NULL;
    ListNode* p;
    char ch;
    // 读入第一个字符
    while ((ch = getchar()) != '\n') {
        if (ch == ' ' || ch == '\t') {
            // 跳过空格和制表符
            continue;
        }
        // 申请新结点
        p = (ListNode*)malloc(sizeof(ListNode));
        p->data = ch;
        p->next = head;
        head = p;
    }
    return head;
}
```

尾插法建表：

```c
// 尾插法建立单链表
LinkList CreateListR() {
    LinkList head = NULL;
    LinkList rear = NULL;
    ListNode* p;
    DataType ch;
    while ((ch = getchar()) != '\n') {
        if (ch == ' ' || ch == '\t') {
            // 跳过空格和制表符
            continue;
        }
        p = (ListNode*)malloc(sizeof(ListNode));
        p->data = ch;
        p->next = NULL;
        if (head == NULL) {
            head = p;
            rear = p;
        }
        else {
            rear->next = p;
            rear = p;
        }
    }
    return head;
}
```

尾插法建表（带一个头结点）：

```c
// 引入头结点后的尾插法建立单链表
LinkList CreateListR1() {
    LinkList head = (ListNode*)malloc(sizeof(ListNode));
    ListNode* p, * r;
    DataType ch;
    r = head;
    while ((ch = getchar()) != '\n') {
        if (ch == ' ' || ch == '\t') {
            // 跳过空格和制表符
            continue;
        }
        p = (ListNode*)malloc(sizeof(ListNode));
        p->data = ch;
        r->next = p;
        r = p;
    }
    r->next = NULL;
    return head;
}
```

**单链表的查找** 有两种，按结点值查找和按序号查找

按结点值查找：

```c
// 按结点值查找
ListNode* LocateNodek(LinkList head, DataType k) {
    // head 为带头结点的单链表的指针，k 为要查找的结点值
    ListNode* p = head->next;
    while (p && p->data != k)
        p = p->next;
    return p;
}
```

按结点序号查找：

```c
// 按结点序号查找
ListNode* GetNodei(LinkList head, int i) {
    // head 为带头结点的单链表的头指针，i 为要查找的结点序号
    ListNode* p = head->next;
    int j = 1;
    while (p != NULL && j < i) {
        p = p->next; ++j;
    }
    if (j == i)
        return p;
    else
        return NULL;
}
```

