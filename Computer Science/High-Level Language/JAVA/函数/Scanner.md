# 导入包

```Java
import java.util.Scanner;
```

# 函数

|方法名|解释说明|
|---|---|
|nextByte()|获取用户从控制台输入的整数，若输入的不是整数，则会出错|
|nextShort()|获取用户从控制台输入的整数，若输入的不是整数，则会出错|
|nextInt()|获取用户从控制台输入的整数，若输入的不是整数，则会出错|
|nextLong()|获取用户从控制台输入的整数，若输入的不是整数，则会出错|
|nextFloat()|获取用户从控制台输入的浮点数，若输入的不是浮点数，则会出错|
|nextDouble()|获取用户从控制台输入的浮点数，若输入的不是浮点数，则会出错|
|nextBoolean()|获取用户从控制台输入的 boolean 值，若输入的不是 true/false，则会出错|
|next()|获取用户从控制台输入的字符串|

## 示例

```Java
Scanner sc = new Scanner(System.in); // 固定写法
System.out.print("请输入你的名字: ");
String name = sc.next(); // 获取用户输入的姓名，并存入变量name中
System.out.println(name);
```