---
tags:
  - 线性表
---
# Definition/定义

如果一个数据元素序列满足以下要求：

1. 第一个数据前没有数据，最后一个数据后面没有数据；
2. 除去最前面和最后面两个数据，其他数据都有唯一的**前驱数据**和唯一的**后继数据**；

则称这些数据组成的结构是线性结构，其构成了**线性表**。

也说，这些数据之间的逻辑关系是线性的。

# **存储结构**

## 顺序存储

组成：数组 => 顺序表

优点：随机存取

缺点：插入&删除需要大量操作，可能会空间浪费

应用场景：随机查看的操作较多，且修改操作较少

### 顺序表

结构体组成：数组(存储数据) + 理论容量 + 实际容量

```C
typedef struct arraylist
{
	int* data;//指针模拟开数组
	int length;//理论容量
	int size;//实际容量
}MyArr;
```

```C
//初始化
MyArr initArray(int n)
{
	MyArr ar;
	ar.data = (int*)malloc(sizeof(int) * n);
	if (ar.data == NULL)
		printf("空间分配失败\n");
	ar.length = n;
	ar.size = 0;
	return ar;
}
```

操作：增 插 删 改 查

1. 增加：在末尾添加一个元素 （p.s. 首先判断是否超限）

```C
//增加
void add(MyArr* a, int k)
{
	if (a->size < a->length)//判断是否超限
	{
		a->data[a->size] = k;
		a->size++;
	}
	else
		printf("空间已满\n");
}
```

1. 插入：在指定位置插入一个元素

```C
//插入
void insert(MyArr* a, int i, int k)
{
	if (a->size < a->length)//判断是否超限
	{
		for (int j = a->size - 1; j >= i; j--)//移动位置(倒着移)
			a->data[j + 1] = a->data[j];
		a->data[i] = k;//插入赋值
		a->size++;
	}
	else
		printf("空间已满\n");
}
```

1. 查找

```C
//查找
int find(MyArr a, int k)
{
	for (int i = 0; i < a.size; i++)
		if (a.data[i] == k)
			return i;
	return -1;
}
```

1. 删除

```C
//删除
void del(MyArr* a, int k)
{
	int i = find(*a, k);
	if (i == -1)
		printf("不存在该数据\n");
	else
	{
		for (int j = i; j < a->size - 1; j++)
			a->data[j] = a->data[j + 1];
		a->size--;
	}
}
```

1. 修改

```C
//修改
void change(MyArr* a, int i, int k)
{
	a->data[i] = k;
}
```

  

## 链式存储

组成：链表（由若干个结点组成）

分类：单向链表、双向链表、循环链表、空链表

相关概念

结点 / 节点：数据 + 下一个数据的地址（指向下一个数据的指针）

头指针：存储第一个结点的地址的指针，可标记一个链表

首元结点：第一个存储真实数据的结点（非空链表一定有）

头结点：链表中第一个没有存储真实数据的结点（可有可无）

作用

1）当不带头结点时，对首元结点的操作需要涉及头指针，非常特殊；带头结点时，首元结点与其他结点无异

2）对空链表：带头结点，空链表和非空链表都有结点

声明链表 = 声明结点

应用场景：1）可以实现各种各样的数据结构，灵活

缺点：不能随机访问

  

p.s.针对链表的代码一律默认链表有头结点；

  

### 单向链表

结构体组成：数据域data + 指针域next（下一个数据的地址）

```C
typedef struct ListNode {
	int data; // 数据
	struct ListNode* next; // 保存下一个结点的地址
	// 用linklist声明头指针 -> 增强可读性
}Node, * linklist;
```

操作：增 插 删 改 查

1. 增加
    
    1）不带头结点的链表：在首元节点前插入一个结点——动头指针；在其他结点前插入结点。
    
    2）带头结点的链表：不分类
    

-步骤

1） 把数据放到一个链表中；

2） 再把结点插入到链表中；

-分类

①头插

```C
//头插
linklist head_insert(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		s->next = head->next;
		head->next = s;
	}
	return head;
}
```

②中插

```C
//中间插
linklist mid_insert(linklist head, int x, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		//先找到x所在的结点
		Node* t = head->next;//比较数据，故从首元节点开始比较
		while (t->data != x)
			t = t->next;
		//插入
		s->next = t->next;
		t->next = s;
	}
	return head;
}
```

③尾插

```C
//尾插
linklist tail_insert(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		s->next = NULL;
		//先找到最后一个结点
		Node* t = head;
		while (t != NULL)
		{
			if (t->next == NULL)
				break;
			else
				t = t->next;
		}
		//插入
		t->next = s;
	}
	return head;
}
```

1. 删除：删除指定数据k
    
    1）找到数据k的位置；
    
    2）进行删除操作；
    

```C
//删除：删除数据k
linklist del(linklist head, int k)
{
	Node* t = head->next;
	Node* p = head;
	while (t->data != k)
	{
		t = t->next;
		p = p->next;
	}
	p->next = t->next;
	t->next = NULL;//可有可无
	free(t);//把t的空间回收；释放空间
	t = NULL;
	return head;
}
```

1. 修改：把指定位置的元素k改成另一个元素x

```C
//修改
linklist change(linklist head, int k, int x)
{
	Node* t = head->next;
	while (t->data != k)
		t = t->next;
	t->data = x;
}
```

  

### 双向链表

结构体组成：数据域data + 指针域pre（上一个数据的地址）+ 指针域next（下一个数据的地址）

```C
typedef struct ListNode {
	struct ListNode* pre;//上一个结点的地址
	int data;
	struct ListNode* next;
}Node, * linklist;
```

```C
//初始化
linklist initlist()
{
	linklist l = (Node*)malloc(sizeof(Node));
	if (l == NULL)
		printf("分配失败\n");
	else
	{
		l->pre = NULL;
		l->next = NULL;
	}
	return l;
}
```

操作：增 插 删 改 查

1. 增加

①头插

```C
//头插
linklist head_insert(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		s->next = head->next;
		s->pre = head;
		if (s->next!=NULL)
			head->next->pre = s;
		head->next = s;
	}
	return head;
}
```

②中插

```C
//中间插
linklist mid_insert(linklist head, int x, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		Node* t = head;
		while (t != NULL && t->data != x)
			t = t->next;
		s->next = t->next;
		s->pre = t;
		t->next->pre = s;
		t->next = s;
	}
	return head;
}
```

③尾插

```C
//尾插
linklist tail_insert(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		s->next = NULL;
		Node* t = head;
		while (t != NULL)
		{
			if (t->next == NULL)
				break;
			t = t->next;
		}
		t->next = s;
		s->pre = t;
	}
	return head;
}
```

1. 删除：删除指定数据k
    
    1）找到数据k的位置；
    
    2）进行删除操作；
    

```C
//删
linklist del(linklist head, int x)
{
	Node* t = head->next;
	while (t != NULL && t->data != x)
		t = t->next;
	t->pre->next = t->next;
	if (t->next != NULL)
		t->next->pre = t->pre;
	t->pre = NULL;
	t->next = NULL;
	free(t);
	t = NULL;
	return head;
}
```

1. 修改：把指定位置的元素k改成另一个元素x

```C
//改
linklist change(linklist head, int x, int x0)
{
	Node* s = head->next;
	while (s != NULL && s->data != x)
		s = s->next;
	s->data = x0;
}
```

  

### 循环单链表

结构体组成：数据域data + 指针域next（下一个数据的地址）

```C
typedef struct NodeList {
	int data;
	struct NodeList* next;
}Node, * linklist;
```

```C
//初始化
linklist init()
{
	linklist l = (Node*)malloc(sizeof(Node));
	if (l == NULL)
		printf("分配失败\n");
	else
		l->next = l;
	return l;
}
```

操作：增 插 删 改 查

1. 增加

①头插

```C
//头插
linklist insert_head(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("分配失败\n");
	else
	{
		s->data = k;
		s->next = head->next;
		head->next = s;
	}
	return head;
}
```

②中插

```C
//中间插
linklist insert_mid(linklist head, int x, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("失败\n");
	else
	{
		s->data = k;
		Node* t = head;
		while (t != NULL)
		{
			if (t->data == x)
				break;
			t = t->next;
		}
		s->next = t->next;
		t->next = s;
	}
	return head;
}
```

③尾插

```C
//尾插
linklist insert_tail(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("失败\n");
	else
	{
		s->data = k;
		s->next = head;
		Node* t = head;
		while (t != NULL)
		{
			if (t->next == head)
				break;
			t = t->next;
		}
		t->next = s;
	}
	return head;
}
```

1. 删除

```C
//删除
linklist del(linklist head, int k)
{
	Node* t = head->next;
	Node* p = head;
	while (t != NULL)
	{
		if (t->data == k)
			break;
		t = t->next;
		p = p->next;
	}
	p->next = t->next;
	t->next = NULL;
	free(t);
	t = NULL;
	return head;
}
```

1. 修改

```C
//改变
linklist change(linklist head, int x, int k)
{
	Node* t = head->next;
	while (t != NULL)
	{
		if (t->data == x)
			break;
	}
	t->data = k;
	return head;
}
```

### 循环双链表

结构体组成：数据域data + 指针域pre（上一个数据的地址）+ 指针域next（下一个数据的地址）

```C
typedef struct NodeList {
	struct NodeList* pre;
	int data;
	struct NodeList* next;
}Node, * linklist;
```

```C
//初始化
linklist init()
{
	linklist l = (Node*)malloc(sizeof(Node));
	if (l == NULL)
		printf("失败\n");
	else
	{
		l->pre = l;
		l->next = l;
	}
	return l;
}
```

操作：增 插 删 改 查

1. 增加

①头插

```C
//头插
linklist insert_head(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("失败\n");
	else
	{
		s->data = k;
		s->next = head->next;
		s->pre = head;
		head->next->pre = s;
		head->next = s;
	}
	return head;
}
```

②中插

```C
//中间插
linklist insert_mid(linklist head, int x, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("失败\n");
	else
	{
		s->data = k;
		Node* t = head;
		while (t != NULL)
		{
			if (t->data == x)
				break;
			t = t->next;
		}
		s->pre = t;
		s->next = t->next;
		t->next->pre = s;
		t->next = s;
	}
	return head;
}
```

③尾插

```C
//尾插
linklist insert_tail(linklist head, int k)
{
	Node* s = (Node*)malloc(sizeof(Node));
	if (s == NULL)
		printf("失败\n");
	else
	{
		s->data = k;
		s->next = head;
		s->pre = head->pre;
		head->pre->next = s;
		head->pre = s;
	}
	return head;
}
```

1. 删除

```C
//删除
linklist del(linklist head, int x)
{
	Node* t = head->next;
	while (t != NULL)
	{
		if (t->data == x)
			break;
		t = t->next;
	}
	t->pre->next = t->next;
	t->next->pre = t->pre;
	t->next = NULL;
	t->pre = NULL;
	free(t);
	t = NULL;
	return head;
}
```

1. 修改

```C
//改变
linklist change(linklist head, int x, int k)
{
	Node* t = head->next;
	while (t != NULL)
	{
		if (t->data == x)
			break;
	}
	t->data = k;
	return head;
}
```