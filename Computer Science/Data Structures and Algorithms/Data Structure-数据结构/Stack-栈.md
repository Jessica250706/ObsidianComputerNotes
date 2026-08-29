# Definition/定义

**栈/堆栈：**受限线性表（能且仅能在一段进行操作）

允许进行操作的一段为 **栈顶**，另一端为 **栈底**

**空栈：**栈中无元素

性质：先进后出 / 后进先出

## 应用

1. 倒着走的逻辑：撤销 / 历史记录
2. 递归：函数的调用基于栈
3. 物理内存
4. C++：STL <stack>

# 存储结构

## 顺序栈

### 缺陷

1. 上溢：当栈满后，继续入栈，产生上溢；
2. 下溢：当栈空后，继续出栈，产生下溢；

### 结构体构成

数据（指针模拟开数组） + 指针（以下标索引形式表示）

```C
typedef struct sstack {
	int* data;
	int top;
}mystack;
// top的初始值有-1与0两种可能，故有两种写法
```

### 写法一：top=-1

```C
//初始化
mystack init(int n)
{
	mystack s;
	s.data = (int*)malloc(n * sizeof(int));
	if (s.data == NULL)
	{
		printf("失败\n");
		exit(0);
	}
	s.top = -1;
	return s;
}
```

1. 入栈：向栈中放入一个数据

```C
//入栈
void ppush(mystack* s, int k,int n)
{
	if (s->top == n - 1)//判满
		printf("栈满\n");
	else
	{
		s->top++;
		s->data[s->top] = k;
		// 上述两句可替换成
		// s->data[++s->top] = k;
	}
}
```

1. 出栈：删除一个数据

```C
//出栈
void ppop(mystack* s)
{
	if (s->top == -1)//判空
		printf("栈空\n");
	else
		s->top--;
}
```

1. 查看栈顶元素的值

```C
//查看栈顶元素
int gget(mystack s)
{
	if (s.top == -1)//判空
	{
		printf("栈空\n");
		return -1;//前提是栈中元素大于等于0
	}
	else
		return s.data[s.top];
}
```

### 写法二：top=0

```C
//初始化
mystack init(int n)
{
	mystack s;
	s.data = (int*)malloc(n * sizeof(int));
	if (s.data == NULL)
	{
		printf("失败\n");
		exit(0);
	}
	s.top = 0;
	return s;
}
```

1. 入栈：向栈中放入一个数据

```C
//入栈
void ppush(mystack* s, int k, int n)
{
	if (s->top == n - 1)//判满
		printf("栈满\n");
	else
	{
		s->top++;
		s->data[s->top] = k;
		// 上述两句可替换成
		// s->data[++s->top] = k;
	}
}
```

1. 出栈：删除一个数据

```C
//出栈
void ppop(mystack* s)
{
	if (s->top == 0)//判空
		printf("栈空\n");
	else
		s->top--;
}
```

1. 查看栈顶元素的值

```C
//查看栈顶元素
int gget(mystack s)
{
	if (s.top == 0)//判空
	{
		printf("栈空\n");
		return -1;//前提是栈中元素大于等于0
	}
	else
		return s.data[s.top];
}
```

## 链式栈

### 缺点

1. 下溢：当栈空后，继续出栈，产生**下溢**；

### 结构体组成

数据 + 指针

```C
typedef struct sstack {
	int data;
	struct sstack* next;
}mystack;
```

### 操作

```C
//初始化
mystack* init()
{
	mystack* s = (mystack*)malloc(sizeof(mystack));
	if (s == NULL)
	{
		printf("失败\n");
		exit(0);
	}
	else
	{
		s->next = NULL;
		return s;
	}
}
```

1. 入栈：向栈中放入一个数据

```C
//出栈
void ppop(mystack* top)
{
	if (top->next == NULL)//判空
		printf("空\n");
	else
	{
		mystack* s = top->next;
		top->next = s->next;
		s->next = NULL;
		free(s);
		s = NULL;
	}
}
```

1. 出栈：删除一个数据

```C
//出栈
void ppop(mystack* top)
{
	if (top->next == NULL)//判空
		printf("空\n");
	else
	{
		mystack* s = top->next;
		top->next = s->next;
		s->next = NULL;
		free(s);
		s = NULL;
	}
}
```

1. 查看栈顶查看栈顶元素的值

```C
//查看
void gget(mystack* top)
{
	if (top->next == NULL)//判空
		printf("空\n");
	else
		printf("%d\n", top->next->data);
}
```