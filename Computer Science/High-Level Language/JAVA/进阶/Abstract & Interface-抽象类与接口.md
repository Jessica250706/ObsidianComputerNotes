# 抽象类 -Abstract

## 概念

> An abstract class is a class that is declared abstract—it may or may not include abstract methods. Abstract classes cannot be instantiated, but they can be subclassed.

> 抽象类是被声明为 abstract 的类 - 它可能包含也可能不包含抽象方法。 抽象类不能被实例化，但是可以被子类化（也就是可以**被继承**）。

### 抽象类定义语法

```Java
//定义一个抽象类
public abstract class 类名{
 
}
```

### 抽象方法定义语法

```Java
访问修饰符 abstract 返回值类型 方法名(参数列表);
//知道要做一件事情，但不知道具体怎么去做这件事情
```

## 举例

```Java
package ex2;

public abstract class Animal {

    // 抽象方法无方法体
    public abstract void eat(); // 动物吃东西，但不知道怎么吃

}
```

```Java
package ex2;

public class Cat extends Animal{

    @Override
    public void eat() {
        System.out.println("cat eat");
    }

}
```

1. 如果一个类继承于一个抽象类，那么该类必须实现这个抽象类中的所有抽象方法。否则，该类必须定义抽象类。
2. 抽象类不一定有抽象方法，但有抽象方法的类一定是抽象类。

# 接口 -Interface

## 概念

在软件工程中，软件与软件的交互很重要，这就需要一个约定。每个程序员都应该能够编写实现这样的约定。接口就是对约定的描述。

> In the Java programming language, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces.
> 在 Java 编程语言中，接口是**类似于类的引用类型**，它只能包含**常量**，**方法签名**，**默认方法**，**静态方法**和**嵌套类型**。 方法主体仅适用于默认方法和静态方法。 接口无法实例化——它们只能由类实现或由其他接口扩展。
> 
> An interface declaration can contain method signatures, default methods, static methods and constant definitions. The only methods that have implementations are default and static methods.
> 接口声明可以包含**方法签名**，**默认方法**，**静态方法**和**常量定义**。 具有实现的方法是默认方法和静态方法。

从上面的描述中可以得出： 接口中**没有构造方法**。

## 定义

### 语法

```Java
[public] interface 接口名 {
    // 接口中定义变量，该变量是*静态常量*，在定义的时候必须赋值
    [public static final] 数据类型 变量名 = 变量的值;

    // 定义接口方法
    返回值类型 方法名([参数列表]);

    // 接口中定义的默认方法，必须在JDK8及以上版本使用
    default 返回值类型 方法名([参数列表]) {
        [return 返回值;]
    }

    // 接口中定义的静态方法，必须在JDK8及以上版本使用
    static 返回值类型 方法名([参数列表]) {
        [return 返回值;]
    }

    // 接口中定义的私有方法，必须在JDK9及以上版本使用
    private 返回值类型 方法名([参数列表]) {
        [return 返回值;]
    }
}
```

```Java
interface Test {
    // 接口中定义变量，该变量是静态常量，在定义的时候必须赋值
    int number = 10;

    // 定义接口方法
    void show();

    // 接口中定义的默认方法，必须在JDK8及以上版本使用，默认为公开
    default int plus(int a, int b) {
        return a + b;
    }

    // 接口中定义的静态方法，必须在JDK8及以上版本使用，默认为公开
    static int multiply(int a, int b) {
        return a * b;
    }

    // 接口中定义的私有方法，必须在JDK9及以上版本使用
    private String getName() {
        return "admin";
    }
}
```

### 举例

```Java
public interface Person {
    String getName();
}
```

## 接口继承

### 语法

```Java
// 多继承
[public] interface 接口名 extends 接口名1, 接口名2,...接口名n {
    
}
```

### 举例

```Java
public interface Actor extends Person{
    void perform(); // 演员表演
}
```

```Java
public interface Singer extends Person{
    void sing(); // 唱歌
}
```

```Java
public interface Artist extends Actor, Singer{
    void endorsement(); // 代言
}
```

p.s. 接口可以**多继承**，这是 Java 中**唯一**可以使用多继承的地方。接口包含的变量都是静态常量，接口中包含的方法签名都是公开的抽象方法，接口中的默认方法和静态方法在 JDK8 及以上版本才能定义，接口的私有方法必须在 JDK9 及以上版本才能定义。**接口编译完成后也会生成相应的 class 文件。**

## 接口实现

### 实现接口语法

```Java
访问修饰符 class 类名 implements 接口名1, 接口名2,...接口名n {
    
}
```

> A class that implements an interface must implement all the methods declared in the interface.

> 实现接口的类必须实现接口中声明的所有方法。

p.s. 一个类如果实现了一个接口，那么就必须实现这个接口中定义的所有抽象方法（包括接口通过继承关系继承过来的抽象方法），这个类被称为接口的**实现类**或者说**子类**。与继承关系一样，实现类与接口之间的关系是 is-a 的关系。

### 举例

```Java
public class EntertainmentStar implements Artist{

    public String name;

    public EntertainmentStar(String name) {
        this.name = name;
    }

    @Override
    public void endorsement() {
        System.out.printf("娱乐明星%s代言\n", getName());
    }

    @Override
    public void perform() {
        System.out.printf("娱乐明星%s表演\n", getName());
    }

    @Override
    public void sing() {
        System.out.printf("娱乐明星%s唱歌\n", getName());
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

```Java
import java.util.EmptyStackException;

public class PersonTest {
    public static void main(String[] args) {
        Person p = new EntertainmentStar("Jessica");
        System.out.println(p.getName());

        Actor a = new EntertainmentStar("Anaxa");
        a.perform();

        Singer s = new EntertainmentStar("Phainon");
        s.sing();

        Artist at = new EntertainmentStar("FangShiqian");
        System.out.println(at.getName());
        at.perform();
        at.sing();
        at.endorsement();
    }
}
```

## 接口应用场景

一般来说，定义规则、定义约定时使用接口。

### 举例

计算机对外暴露有 USB 接口，USB 接口生产商只需要按照接口的约定生产相应的设备 (比如 USB 键盘、USB 鼠标、优盘) 即可。

```Java
public interface USB {
    void service(); // USB接口
}
```

```Java
public class UDisk implements USB {
    @Override
    public void service() {
        System.out.println("已接入优盘，可存储数据");
    }
}
```

```Java
public class KeyBoard implements USB{
    @Override
    public void service() {
        System.out.println("已接入键盘，可开始打字");
    }
}
```

```Java
public class Mouse implements USB{
    @Override
    public void service() {
        System.out.println("已接入鼠标，可移动光标");
    }
}
```

```Java
public class Computer {

    // 一台电脑拥有4个USB接口
    private USB[] usbArr = new USB[4];

    // 插入USB接口
    public void insertUSB(int index, USB usb) {
        if (index < 0 || index >= usbArr.length) {
            System.out.println("Invalid index");
        }
        else {
            usbArr[index] = usb;
            usb.service();
        }
    }

}
```

```Java
public class ComputerTest {

    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.insertUSB(1, new Mouse());
    }

}
```

# 总结

## 抽象类和接口的区别

1. 抽象类拥有构造方法，而接口没有构造方法；
2. 抽象类可以定义成员变量、静态变量、静态常量，而接口中只能定义公开的静态常量；
3. 抽象类中的方法可以有受保护、默认的方法，而接口中的方法都是公开的（JDK9 中可以定义的私有方法除外）；
4. 抽象类主要应用在对于**抽象事物**的描述（is-a），而接口主要应用在对于**约定、规则**的描述（has-a）；
5. 抽象类只能够单继承，而接口可以多继承；