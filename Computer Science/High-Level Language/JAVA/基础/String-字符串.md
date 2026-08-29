# 介绍

## 特性

1. String 类位于 java.lang 包中，无需引入，直接使用即可。
2. String 类是由 final 修饰的，表示 String 类是一个最终类，不能够被继承。
3. String 类构建的对象不可再被更改。（be String 类表示一个字符串，而字符串中的字符使用的是一个 final 类修饰 char 数组存储）

## 构造方法

```Java
public String(String original); 

public String(char value[]);

public String(char value[], int offset, int count);

public String(byte bytes[]);

public String(byte bytes[], int offset, int length);

public String(byte bytes[], Charset charset);
```

p.s. **当使用一个字面量给字符串赋值时，首先会去字符串常量池中检测是否存在这个字面量。**若存在，则直接使用这个字面量的地址赋值即可。若不存在，则需要在字符串常量池中创建这个字面量，然后再将地址赋值过去。（字符串拼接动作发生在堆内存上）

![[image 15.png|image 15.png]]

### 举例

```Java
import java.nio.charset.Charset;

public class StringExample {

    public static void main(String[] args) {
        // 创建两个对象：
        // 1.字面量会在常量池中创建一个对象
        // 2.new String("") 构造方法创建出来的对象
        String str1 = new String("Phainon");
        System.out.println(str1); // Phainon

        char[] values = {'a', 'b', 'c', 'd', 'e', 'f'};
        String str2 = new String(values);
        System.out.println(str2); //abcdef

        // offset:偏移量 count:个数
        // 需要考虑数组下标越界的可能
        String str3 = new String(values, 1, 3);
        System.out.println(str3); // bcd

        // 字节可以存储整数，字符也可以使用整数表示，这个整数就是ASCII码对应的整数值
        byte[] bytes = {97, 98, 99, 100, 101, 65};
        String str4 = new String(bytes);
        System.out.println(str4); // abcdeA

        // offset:偏移量 length:长度
        String str5 = new String(bytes, 1, 3);
        System.out.println(str5); // bcd

        Charset charset = Charset.forName("UTF-8"); // 构建UTF-8字符集
        String str6 = new String(bytes, charset);
        System.out.println(str6); // abcdeA

    }

}
```

## 常用方法

### 获取长度

```Java
public int length(); // 获取字符串的长度
```

### 字符串比较

```Java
// 比较两个字符串是否相同
public boolean equals(Object anObject);

// 忽略大小比较两个字符串是否相同
public boolean equalsIgnoreCase(String anotherString);
```

### 字符串大小写转换

```Java
// 转换为小写
public String toLowerCase();
// 转换为大写
public String toUpperCase();
```

### 获取字符在字符串中的下标

```Java
// 获取指定字符在字符串中第一次出现的下标
public int indexOf(int ch); 
// 获取指定字符在字符串中最后一次出现的下标
public int lastIndexOf(int ch);
```

### 获取字符串在字符串中的下标

```Java
//获取指定字符串在字符串中第一次出现的下标
public int indexOf(String str);
//获取指定字符串在字符串中最后一次出现的下标
public int lastIndexOf(String str);
```

### 获取字符串中的指定下标的字符

```Java
 public char charAt(int index);
```

### 字符串截取

```Java
// 从指定开始位置截取字符串，直到字符串的末尾
public String substring(int beginIndex);
// 从指定开始位置到指定结束位置截取字符串（左闭右开）
// [beginIndex, endIndex)
public String substring(int beginIndex, int endIndex);
```

### 字符串替换

```Java
// 使用新的字符替换字符串中存在的旧的字符
public String replace(char oldChar, char newChar);
// 使用替换的字符串来替换字符串中的就的字符串
public String replace(CharSequence target, CharSequence replacement);

// 使用替换的字符串来替换字符串中满足正则表达式的字符串
public String replaceAll(String regex, String replacement);
```

### 获取字符数组

```Java
 public char[] toCharArray(); 
```

### 获取字节数组

```Java
// 获取字节数组
public byte[] getBytes();
// 获取指定编码下的字节数组
public byte[] getBytes(Charset charset);
```

### 字符串拼接

```Java
// 将字符串追加到末尾
public String concat(String str); 
```

### 去除字符串两端的空白字符

```Java
public String trim();
```

### 字符串分割

```Java
// 将字符串按照匹配的正则表达式分割
public String[] split(String regex);
```

```Java
public class StringExample2 {
    public static void main(String[] args) {
        String str = "a1b2c3d4e5";
        String[] arr = str.split("[0-9]");
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        /*
        结果：
        a
        b
        c
        d
        e
         */
    }
}
```

```Java
public class StringExample2 {
    public static void main(String[] args) {
        String str = "Phainon,male,21";
        String[] arr = str.split(",");
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        /*
        结果：
        Phainon
        male
        21
         */
    }
}
```

### 字符串匹配正则表达式

```Java
// 检测字符串是否匹配给定的正则表达式
public boolean matches(String regex); 
```

### intern() 方法

```Java
// 将字符串s放入字符串常量池，放入时会先检测常量池中是否存在该字符串s。
// 若字符串常量池中存在该字符串s，那么新赋值的字符串s0会直接使用常量池中的s字符串地址。
// 若不存在，则在常量池中创建一个字符串s。
public native String intern();
```

# StringBuilder 和 StringBuffer

## 特性介绍

1. StringBuilder 类位于 java.lang 包中，无需引入，直接使用即可。
2. StringBuilder 类是由 final 修饰的，表示 StringBuilder 是一个最终类，不可能被继承。
3. StringBuilder 类构建的对象，可以实现字符序列的追加，但不会产生新的对象，只是将这个字符序列保存在字符数组中。

## 构造方法

```Java
// 构建一个StringBuilder对象，默认容量为16
public StringBuilder(); 

// 构建一个StringBuilder对象并指定初始化容量
public StringBuilder(int capacity);

// 构建一个StringBuilder对象，并将指定的字符串存储在其中
public StringBuilder(String str); 
```

### 举例

```Java
public class StringBuilderExample {

    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder(); // 构建了一个初始化容量为16的字符串构建器
        StringBuilder sb2 = new StringBuilder(1024); // 构建了一个初始化容量为1024的字符串构建器
        StringBuilder sb3 = new StringBuilder("Hello"); // 长度：16 + 5
    }

}
```

## 常用方法

### 追加

```Java
// 将一个字符串添加到StringBuilder存储区
public StringBuilder append(String str);

// 将StringBuffer存储的内容添加StringBuilder存储区
public StringBuilder append(StringBuffer sb);
```

### 删除指定区间存储的内容

```Java
// 将StringBuilder存储区指定的开始位置到指定的结束位置之间的内容删除掉
// [start, end)
public StringBuilder delete(int start, int end);
```

### 删除存储区指定下标位置存储的字符

```Java
public StringBuilder deleteCharAt(int index);
```

### 在 StringBuilder 存储区指定偏移位置处插入指定的字符串

```Java
public StringBuilder insert(int offset, String str);
```

### 将存储区的内容倒序

```Java
public StringBuilder reverse();
```

### 获取指定字符串在存储区中的位置

```Java
// 获取指定字符串在存储区中第一次出现的位置
public int indexOf(String str); 
// 获取指定字符串在存储区中最后一次出现的位置
public int lastIndexOf(String str);
```

## 比较：String、StringBuilder 和 StringBuffer

String、StringBuilder 和 StringBuffer 都是用来处理字符串的。在处理**少量字符串**的时候，它们之间的处理效率几乎没有任何区别。

但在处理**大量字符串**的时候，由于 String 类的对象不可再更改，因此在处理字符串时会产生新的对象，对于内存的消耗来说较大，导致效率低下。

而 StringBuilder 和 StringBuffer 使用的是对字符串的字符数组内容进行拷贝，不会产生新的对象，因此效率较高。（对内存上的开销拉说，相比于 String 类减少了很多。）

而 StringBuffer 为了保证在多线程情况下字符数组中内容的正确（同步）使用，在每一个成员方法上面加了锁，有锁就会增加消耗，因此 StringBuffer 在处理字符串的效率上要略低于 StringBuilder。（处理少量字符串时，其效率几乎无差别。）