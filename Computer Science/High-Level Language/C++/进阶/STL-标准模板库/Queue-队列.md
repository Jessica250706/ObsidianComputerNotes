# 定义

```C++
#include <queue>
```

# 初始化



# 元素访问



# 常用函数

| 函数声明    | 接口说明                       |
| ------- | -------------------------- |
| queue() | 构造空的队列                     |
| empty() | 检测队列是否为空，是返回true,否则返回false |
| size()  | 返回队列中有效元素的个数               |
| front() | 返回队头元素的引用                  |
| back()  | 返回队尾元素的引用                  |
| push()  | 在队尾将元素val入队列               |
| pop()   | 将队头元素出队列                   |

# 衍生

## 优先队列

### 1 头文件

```C++
#include <queue>
```

### 2 定义

```C++
// priority_queue<结构类型> 队列名;
priority_queue <int> i;
priority_queue <double> d;

// 常见

priority_queue <node> q;
//node是一个结构体
//结构体里重载了‘<’小于符号

//不需要#include<vector>头文件
//注意后面两个“>”不要写在一起，“>>”是右移运算符
priority_queue <int,vector<int>,greater<int> > q; // 从小到大
priority_queue <int,vector<int>,less<int> >q; // 从大到小
```

### 3 函数

| 函数声明      | 接口说明                |
| --------- | ------------------- |
| q.size()  | 返回q里元素个数<br>        |
| q.empty() | 返回q是否为空，空则返回1，否则返回0 |
| q.push(k) | 在q的末尾插入k            |
| q.pop()   | 删掉q的第一个元素           |
| q.top()   | 返回q的第一个元素           |

