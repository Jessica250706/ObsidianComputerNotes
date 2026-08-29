```C++
\#include <vector>
```

# 定义

## 一维

```C++
vector<typename> name;
```

其中，typename 可以是**int/double/char/结构体**，也可以是**STL 标准容器**，比如 vector/set/queue

## 二维

```C++
vector<typename> Arrayname[size];
```

其中，Arrayname[] 中的每个元素都是一个 vector

  

# 初始化

## 一维

1. 花括号赋值

```C++
vector<int> table{1, 2, 3};
```

1. 圆括号赋值

```C++
vector<int> table(n, p); // 初始化n个值为p（p默认为0，可不写）
```

  

## 二维

```C++
vector<vector<int>> table(size1, vector<int>(size2, p)); // size1是行，size2是列，p是初始化的数字
```

![[image 36.png|image 36.png]]

  

![[image 1 15.png|image 1 15.png]]

# 元素访问

## 下标访问

```C++
table[0] // 访问
```

## 迭代器访问

```C++
for(vector<int>::iterator it = v.begin(); it != v.end(); it++)
	cout << *it << " ";
```

# 常用函数

| 函数                             | 功能                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------------------------ |
| table.begin()                  | 返回 table 首元素下标                                                                               |
| table.end()                    | 返回 table 最后一个元素的**后一位**下标                                                                    |
| table.size()                   | 获取 table 中元素的个数，O(1)，返回值为 unsigned 类型                                                          |
| table.push_back(x)             | 在 vector 容器**末尾**添加一个元素 x，O(1)                                                                |
| table.pop_back()               | 删除 vector 容器**末尾**的元素，O(1)                                                                   |
| table.insert(pos, x)           | 在 vector 容器的 pos 位置添加一个元素 x，并返回表示新插入元素位置的迭代器，O(n)                                               |
| table.insert(pos, n, x)        | 在 pos 指定的位置前插入 n 个元素 x，并返回表示第一个新插入元素位置的迭代器                                                      |
| table.insert(pos, first, last) | 在 pos 位置之前（从下标为 pos 的位置开始），插入其他容器中位于 [first, last) 区域的所有元素（简单说，就是将两个容器拼接在一起），并返回表示第一个新插入元素位置的迭代器 |
| table.insert(pos, initlist)    | 在 pos 指定的位置前插入初始化列表（用{}括起来的多个元素，中间用逗号隔开），并返回表示第一个新插入元素位置的迭代器                                 |
| table.erase(pos)               | 在 vector 容器中删除下标为 pos 的元素                                                                      |
| table.erase(first, last)       | 在 vector 容器中删除下标在 [first, last) 中的元素                                                           |
| table.clear()                  | 清空 table 中的所有元素                                                                              |
| table.resize()                 | 重新定义 table 的大小                                                                             |

[参考](https://www.runoob.com/w3cnote/cpp-vector-container-analysis.html)

# 函数说明

## 增加

### push_back()

1. table.push_back(const T& x)

功能：在 vector 容器**末尾**添加一个元素 x

类型：void

时间复杂度：O(1)

```C++
table.push_back(1); // 在容器末尾加1
```

### insert()

1. table.insert(iterator pos, const T& x)

功能：在 vector 容器的 pos 位置添加一个元素 x，并返回表示新插入元素位置的迭代器

类型：iterator

时间复杂度：O(n)

```C++
table.insert(table.begin() + 2, -1); // 在table下标为2的位置插入-1
```

1. table.insert(iterator pos, int n, const T& x)

功能：在 pos 指定的位置前插入 n 个元素 x，并返回表示第一个新插入元素位置的迭代器

类型：iterator

```C++
table.insert(table.begin() + 2, 2, 5); // 在table下标为2的位置插入2个5
```

1. table.insert(iterator pos, const_iterator first, const_iterator last)

功能：在 pos 位置之前（从下标为 pos 的位置开始），插入其他容器中位于 [first, last) 区域的所有元素（简单说，就是将两个容器拼接在一起），并返回表示第一个新插入元素位置的迭代器

```C++
table.insert(table.end(), v.begin(), v.end()); // v是其他容器（不局限于vector）
```

1. table.insert(iterator pos, initlist)

功能：在 pos 指定的位置前插入初始化列表（用{}括起来的多个元素，中间用逗号隔开），并返回表示第一个新插入元素位置的迭代器

```C++
table.insert(table.begin() + 2, {1, 2}); // 在table下标为2的位置插入1和2
```

  

## 删除

### pop_back()

1. table.pop_back()

功能：删除 vector 容器**末尾**的元素

时间复杂度：O(1)

```C++
table.pop_back();
```

### erase()

1. table.erase(iterator pos)

功能：在 vector 容器中删除下标为 pos 的元素

时间复杂度：O(n)

```C++
table.erase(p);
```

1. table.erase(iterator first, iterator last)

功能：在 vector 容器中删除下标在 [first, last) 中的元素

时间复杂度：O(n)

```C++
table.erase(first, last);
```

p.s. 若想用 erase 函数**模拟 clear 函数**，则

```C++
table.erase(table.begin(), table.end());
```

### clear()

1. table.clear()

功能：清空 table 中的所有元素

```C++
table.clear();
```