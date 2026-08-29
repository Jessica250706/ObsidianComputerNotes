# Encapsulation/封装

## 定义

封装就是将类的部分属性和方法隐藏起来，不允许外部程序直接访问，只能通过该类提供的公开的方法来访问类中定义的属性和方法。封装是面向对象的三大特征之一。

## 做法

1. 先将类中定义的成员属性全部修改为private修饰；
2. 然后对每一个属性提供一个对外访问的方法，即生成gettter/setter方法；
3. 最后在对外访问的方法（gettter/setter）中加入属性值验证；

## 作用

1. 封装提高了代码的重用性。因为封装会提供对外访问的公开的方法，而方法可以重用，因此封装提高了代码的重用性。
2. 封装提高了代码的可维护性。修改代码时，只需要修改部分代码，但不会影响其他代码。
    
    Eg. 年龄在设计时只考虑到了负数的情况，没有考虑实际生活中的情况，人的年龄一般都不会超过200岁，因此还需要加上一层验证
    
3. 封装隐藏了类的具体实现细节，保护了代码实现逻辑。

## 举例

```Java
public class Person {
    // 变量
    private String name; // 姓名
    private int age; // 年龄

    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 获取
    public String getName() {
        return name;
    }

    // 合法性校验
    public void setAge(int age) {
        if (age < 0) {
            System.out.println("输入错误");
        }
        else {
            this.age = age;
        }
    }
    
}
```

# Package/包

## 定义

包是Java中的一个专业词汇，包的本质就是一个文件夹。

## 作用

包可以对我们编写的类进行分类、可以防止命名冲突和访问权限控制。

## 创建

### 语法

```Java
package 包名;
// 如果一个类中有包的定义，那么这个类的第一行有效代码一定是包的定义
```

### 命名规范

1. 一般都是由小写字母和数字组成，每个包之间使用'.'隔开，换言之，每出现一个'.'，就是一个包
2. 一般都含有前缀。比如个人/组织通常都是 _org.姓名_ ，公司通常都是 _com.公司名称简写_ 或者 _cn.公司名称简写_ 。

## 引入

### 语法

```Java
// 类的全限定名：包 + '.' + 类名
import 包名.类名;
```

### 原因

因为JVM只能识别当前包下所有的类，如果要使用当前包之外的其他包中的类，那必须告诉JVM，使用的是哪一个包中的哪一个类。

### 举例

```Java
import java.util.Scanner; // 告诉JVM，到java.util包下去找一个名为Scanner的类

public class Test {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
    }

}
// 一个类同时引用了两个来自不同包的同名类，，必须通过完整类名（类的全限定名）来区分
```

### 常用包说明

1. java.lang包
    
    属于java语言开发包，该包中的类可以直接拿来使用，而不需要引入包。因此JVM会自动引入。比如System、String、Math。
    
2. java.util包
    
    属于java提供的一些实用类以及工具类。比如Scanner。
    

# 访问修饰符

## 定义

访问修饰符就是控制访问权限的修饰符号。

## 类的访问修饰符

1. public修饰符
    
    表示类可以公开访问。
    
2. 默认修饰符（不写修饰符就是默认）
    
    表示该类只能在同一个包中可以访问。
    

## 类成员访问修饰符

类成员包括了成员属性和成员方法。类成员访问修饰符换言之就是**成员属性**和**成员方法**的访问修饰符。

![image 17.png](images/image%2017.png)

## static 修饰符

### 应用范围

static 修饰符只能用来修饰类中定义的成员变量、成员方法、代码块以及内部类（内部类有专门章节进行讲解）。

### 修饰成员变量

static 修饰的成员变量称之为类变量。属于该类所有成员共享。

1. 若类变量是公开的，则可使用_类名.变量名_直接访问该类变量
2. 若类方法是公开的，则可使用_类名.方法名_直接访问该类方法

### 举例

```Java
import java.lang.String;

public class Person {
    // 变量
    private String name; // 姓名
    private int age; // 年龄

    // 使用的static修饰的成员变量称为类变量，不会随着成员变化而变化，属于所有成员共享
    private static String country;

    // static修饰的代码块称为静态代码块，在JVM第一次加载该类的时候执行，只能执行一次
    // 在调用类的构造方法或方法式，会被调用，先于构造方法/方法被调用
    static {
        country = "China";
    }

}
```