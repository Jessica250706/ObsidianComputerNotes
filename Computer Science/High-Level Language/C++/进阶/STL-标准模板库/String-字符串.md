# 头文件

```C++
\#include <string>
```

# 初始化

```C++
string s1; // 默认初始化，一个空字符串s1
string s2(s1); // s2是s1的副本
string s3 = s1; // 等价于s3(s1)，s3是s1的副本

string s4("Hello"); // s4是字面值"hello"
string s5 = "hello"; // s5是字面值"hello"

string s6(n, 'a'); // 把s6初始化为由连续的n个字符c组成的串

// 调用构造函数
string s7 = string("hello");
string s8(string("hello"));
```

# 输入&输出

## cin输入

```C++
string s1;
cin >> s1; // 遇到空格终止
cout << s1 << endl;
```

## getline读取整行

```C++
string s1;
// vs中使用getline，必须加上<string>头文件
getline(cin, s1); // 读取整行
cout << s1 << endl;
```

# 特殊

## 比较大小

string可直接用>,<,==等进行比较

```C++
string s1 = "abc", s2 = "def";
// s1 > s2
// s1 == s2
// s1 < s2
```

## 连接

可直接用“+”相连。

```C++
string s1 = "abc", s2 = "def";
string s3 = s1 + s2;
```

## 获取字符

### C++11新特性的for

```C++
string s1 = "abc";
for (auto c : s1)
{
	cout << c << " ";
}
```

### 运算符[] + size()函数

```C++
s1[0];
```

### 迭代器

```C++
string s1 = "Hello World!";
for (auto i = s1.begin(); i != s1.end(); i++)
	cout << *i << " ";
```

## 拷贝string对象