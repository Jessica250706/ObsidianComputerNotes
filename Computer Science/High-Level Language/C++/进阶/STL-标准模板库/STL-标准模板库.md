Standard Template Library，标准模板库

# Container/容器

[[Computer Science/High-Level Language/C++/进阶/STL-标准模板库/String-字符串|String-字符串]]

[[Vector-向量]]

[[Computer Science/High-Level Language/C++/进阶/STL-标准模板库/List-列表|List-列表]]

[[Computer Science/High-Level Language/C++/进阶/STL-标准模板库/Queue-队列|Queue-队列]]

[[Computer Science/High-Level Language/C++/进阶/STL-标准模板库/Stack-栈|Stack-栈]]

[[Computer Science/High-Level Language/C++/进阶/STL-标准模板库/Set-集合|Set-集合]]

[[Map-映射]]

## 序列容器

功能：存储元素的序列，允许双向遍历。

```C++
std::vector // 动态数组，支持快速随机访问。
std::deque // 双端队列，支持快速插入和删除。
std::list // 链表，支持快速插入和删除，但不支持随机访问。
```

## 关联容器

功能：存储键值对，每个元素都有一个键（key）和一个值（value），并且通过键来组织元素。

```C++
std::set // 集合，不允许重复元素。
std::multiset // 多重集合，允许多个元素具有相同的键。

std::map // 映射，每个键映射到一个值。
std::multimap // 多重映射，存储了键值对（pair），其中键是唯一的，但值可以重复，允许一个键映射到多个值。
```

## 无序容器

功能：哈希表，支持快速的查找、插入和删除。

p.s. C++11 引入。

```C++
std::unordered_set // 无序集合。
std::unordered_multiset // 无序多重集合。

std::unordered_map // 无序映射。
std::unordered_multimap // 无序多重映射。
```

# Iterator/迭代器

## 作用

提供了访问容器中对象的方法。类似指针。

# Algorithm/算法

## 定义

是用来操作容器中的数据的模板函数。