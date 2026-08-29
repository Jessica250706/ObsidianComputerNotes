```C++
\#include <map>
```

# 定义

```C++
map<key_type, value_type> myMap;
// key_type 是键的类型
// value_type 是值的类型
```

# 初始化

## 构造函数

  

## 赋值

  

  

# 元素访问

  

# 常用函数

|函数|功能|
|---|---|
|size()|计算元素个数|
|empty()|判断是否为空，为空则返回true|
|clear()|清空容器|
|erase()|删除元素|
|find()|查找元素|
|insert()|插入元素|
|count()|计算指定元素出现的次数|
|begin()|返回迭代器头部|
|end()|返回迭代器尾部|
|rbegin()|指向map尾部的逆向迭代器|
|rend()|指向map头部的逆向迭代器|
|swap()|交换两个map容器，其类型需要相同|
|max_size()|容纳的最大元素个数|
|lower_bound()|返回键值大于等于指定元素的第一个位置|
|upper_bound|返回键值大于指定元素的第一个位置|
|equal_range()|返回等于指定元素的区间|