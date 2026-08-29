---
tags:
  - 队列
  - 数据结构
---
# Definition/定义

队列：在线性表的基础上，只允许在其一端进行添加，另一端进行删除

队尾：进行添加操作，指向队列的尾部；

队头：进行删除操作，指向队列的第一个元素

性质：先进先出 / 后进后出

# 应用

计算机内部一些需要排序的操作

# 存储结构

## 顺序队列

### 结构体组成

```C
typedef struct queue {
	int front; // 队头指针（以下标索引形式表示）
	int* data; // 数据元素
	int rear; // 队尾指针（以下标索引形式表示）
	int maxsize; // 最大可容纳量
}arrQ;
```

### 方法一：rear=0

```C
//初始化
arrQ init(int n)
{
	arrQ q;
	q.data = (int*)malloc(sizeof(int) * n);
	q.maxsize = n;
	q.front = 0;
	q.rear = 0;
	return q;
}
```

1. 入队

```C
//入队
void enQueue(arrQ* q, int x)
{
	if (q->rear == q->maxsize) // 判满
		printf("队满\n");
	else
	{
		q->data[q->rear] = x;
		q->rear++;
	}
}
```

1. 出队

```C
//入队
void enQueue(arrQ* q, int x)
{
	if (q->rear == q->maxsize) // 判满
		printf("队满\n");
	else
	{
		q->data[q->rear] = x;
		q->rear++;
	}
}
```

### 方法二：rear=-1

```C
//初始化
arrQ init(int n)
{
	arrQ q;
	q.data = (int*)malloc(sizeof(int) * n);
	q.maxsize = n;
	q.front = 0;
	q.rear = -1;
	return q;
}
```

1. 入队

```C
//入队
void enQueue(arrQ* q, int x)
{
	if (q->rear == q->maxsize - 1) // 判满
		printf("队满\n");
	else
	{
		q->data[q->rear] = x;
		q->rear++;
	}
}
```

1. 出队

```C
//出队
void deQueue(arrQ* q)
{
	if (q->rear + 1 == q->front)
		printf("队空\n");
	else
		q->front++;
}
```

## 循环队列

但是，由于顺序队列会造成**假溢出**，即数组前部为空的现象，故引入**循环队列**，增加内存空间的利用率。

### 结构体组成

```C
typedef struct queue {
	int* data; // 队列中的数据元素
	int front; // 队头指针（以索引下标形式表示指针）
	int rear; // 队尾指针（以索引下标形式表示指针）
	int maxsize;
}arrQ;
```

### 写法一：rear=0

```C
//初始化
arrQ init(int n)
{
	arrQ q;
	q.data = (int*)malloc(n * sizeof(int));
	q.maxsize = n;
	q.rear = q.front = 0;
	return q;
}
```

1. 入队

```C
//入队
void enQueue(arrQ* q, int x)
{
	if ((q->rear + 1) % q->maxsize == q->front)//判满
		printf("满\n");
	else
	{
		q->data[q->rear] = x;
		q->rear = (q->rear + 1) % q->maxsize;//队尾指针加一取模
	}
}
```

1. 出队

```C
//出队
void deQueue(arrQ* q)
{
	if (q->front == q->rear)//判空
		printf("空\n");
	else
		q->front = (q->front + 1) % q->maxsize;
}
```

### 写法二：rear=-1

  

## 链式队列

### 结构体组成

```C
// 链式队列结点
typedef struct queueNode {
	int data; // 数据元素
	struct queueNode* next; // 指向下一结点的指针
}qNode, * qLink;

// 链式队列：结构体定义 or 直接定义队头队尾指针
typedef struct linkqueue {
	qLink front; // 队头指针
	qLink rear; // 队尾指针
}linkQueue;
```

### 操作

```C
// 初始化
void init(linkQueue* q)
{
	q->front = q->rear = (qLink)malloc(sizeof(qNode));
	if (q->front == NULL)
		printf("分配失败\n");
	else
		q->front->next = NULL;
}
```

1. 出队

```C
//入队
void enQueue(linkQueue* q, int x)
{
	qLink s = (qNode*)malloc(sizeof(qNode));
	s->data = x;
	s->next = NULL;//新节点插入到链尾
	q->rear->next = s;
	q->rear = s;
}
```

1. 入队

```C
// 出队
// 队首指针是链表头结点，删除的是队首指针的下一个，即front->next
void deQueue(linkQueue* q)
{
	if (q->front->next == NULL) // 判空（或者 q->front == q->rear;）
		printf("空\n");
	else
	{
		qLink p = q->front->next;
		q->front->next = p->next;
		if (q->rear == p) // 若原队列有且仅有一个结点，则删除变空，需要处理尾指针
			q->rear = q->front;
		free(p);
	}
}
```

## 双端队列

概念：线性表的两端都是队列的队头队尾，在线性表的两端都可以执行入队和出队的操作。

优点：通过对不同操作的组合，实现需要的概念/容器。

要求：从左端入队的元素靠左，从右端入队的元素靠右。

结构储存：顺序实现-循环数组

操作：左入队 右入队 左出队 右出队

左/右 入队&出队 => 栈

左 入队/出队 & 右 出队/入队 => 队列