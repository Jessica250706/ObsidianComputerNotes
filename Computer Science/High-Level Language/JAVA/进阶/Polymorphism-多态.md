# Polymorphism/多态

## 概念

> The dictionary definition of polymorphism refers to a principle in biology in which an organism or species can have many different forms or stages. This principle can also be applied to object-oriented programming and languages like the Java language. Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class.

> 多态性的字典定义是指在生物学原理，其中的生物体或物质可具有许多不同的形式或阶段。 该原理也可以应用于面向对象的编程和 Java 语言之类的语言。 一个类的子类可以定义自己的独特行为，但可以共享父类的某些相同功能。

从上面的描述中我们可以得出：继承、接口就是多态的具体体现方式。多态主要体现在类别、做事的方式上面。多态是面向对象的三大特征之一，多态分为编译时多态和运行时多态两大类。

## 编译时多态

**方法重载**在编译时就已经确定如何调用，因此方法重载属于编译时多态。

## 运行时多态

> The Java virtual machine (JVM) calls the appropriate method for the object that is referred to in each variable. It does not call the method that is defined by the variable's type. This behavior is referred to as virtual method invocation and demonstrates an aspect of the important polymorphism features in the Java language.

> Java 虚拟机（JVM）为每个变量中引用的对象调用适当的方法。 它不会调用由变量类型定义的方法。这种行为称为虚拟方法调用，它说明了 Java 语言中重要的多态性特征的一个方面。

主要体现在**方法重写**上，因为只有在程序运行时才知道一个对象的引用到底指向堆内存中的某一个具体对象，从而才能确定调用哪个对象的方法。

## instanceof 运算符

instanceof 本身意思表示的是什么什么的一个实例。主要应用在类型的强制转换上面。在使用强制类型转换时，如果使用不正确，在运行时会报错。而 instanceof 运算符对转换的目标类型进行检测，如果是，则进行强制转换。这样可以保证程序的正常运行。

### 语法

```Java
对象名 instanceof 类名;  
//表示检测对象是否是指定类型的一个实例。返回值类型为boolean类型
```

### 举例

```Java
public abstract class Animal {
    // 抽象方法无方法体
    public abstract void eat(); // 动物吃东西，但不知道怎么吃
}
```

```Java
public class Cat extends Animal{

    @Override
    public void eat() {
        System.out.println("cat eat");
    }

    public void scratch() {
        System.out.println("cat scratch");
    }

}
```

```Java
public class ZooKeeper {

    public void feedAnimal(Animal animal) {
        animal.eat();
        // 如果animal对象是一个Cat类的实例
        if (animal instanceof Cat) {
            ((Cat)animal).scratch();
        }
    }

}
```

# Object 类常用方法

Object 类中定义的方法大多数都是属于 native 方法，native 表示的是本地方法，实现方式是在 C++ 中。

## getClass()

```Java
@IntrinsicCandidate
public final native Class<?> getClass();
// 本地方法
// getClass() 方法返回一个Class对象，该对象具有可用于获取有关该类的信息的方法，例如其名称(getSimpleName())，其超类(getSuperclass())及其实现的接口(getInterfaces())。
```

### 举例

```Java
public class ObjectTest {

    public static void main(String[] args) {
        Cat cat = new Cat();
        Class clazz = cat.getClass();
        System.out.println(clazz.getName()); // 类的全限定名
        System.out.println(clazz.getSimpleName()); // 类名
        Class superClass = clazz.getSuperclass(); // 获取父类的定义信息

        String s = "admin";
        Class stringClass = s.getClass();
        Class[] interfaces = stringClass.getInterfaces();
        for (int i = 0; i < interfaces.length; i++) {
            System.out.println(interfaces[i].getName()); // 接口的全限定名
            System.out.println(interfaces[i].getSimpleName()); // 接口的名称
        }
    }

}
```

## hashCode()

```Java
@IntrinsicCandidate
public native int hashCode();
// hashCode() 返回值是对象的哈希码，即对象的内存地址（十六进制）。
// 根据定义，如果两个对象相等，则它们的哈希码也必须相等。 如果重写equals() 方法，则会更改两个对象的相等方式，并且Object的
// hashCode() 实现不再有效。 因此，如果重写equals() 方法，则还必须重写hashCode() 方法。
```

Object 类中的 hashCode() 方法返回的就是对象的内存地址。一旦重写 hashCode() 方法，那么 Object 类中的 hashCode() 方法就是失效，此时的 hashCode() 方法返回的值不再是内存地址。

### 举例

```Java
// hashCode()方法被重写后，返回的值就不再是对象的内存地址
@Override
public int hashCode() {
    return this.name.hashCode() + this.age;
}
```

## equals(Object obj)

```Java
public boolean equals(Object obj) {
        return (this == obj);
}

// equals()方法比较两个对象是否相等，如果相等则返回true。 Object类中提供的equals()方法使用身份运算符（==）来确定两个对象是否相等。 对于原始数据类型，这将给出正确的结果。 但是，对于对象，则不是。 Object提供的equals()方法测试对象引用是否相等，即所比较的对象是否完全相同。

// 要测试两个对象在等效性上是否相等（包含相同的信息），必须重写equals（）方法。
```

根据定义，如果两个对象相等，则它们的哈希码也必须相等，反之则不然。  
重写了 equals 方法，就需要重写 hashCode 方法，才能满足上面的结论。  

### 举例

```Java
public class Student {

    String name;
    int age;

    public Student() {}
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

		// 若两个对象相等，则它们的哈希码一定相等，反之则不成立
		
		// 若重写了equals方法，那么一定要重写hashCode方法。
		// 因为，若不重写hashCode方法，就会调用Object类中的hashCode方法，得到的是内存地址。不同对象的内存地址是不一致的，但是equals方法后重写后，比较的不是内存地址，而是对象的内部信息，这就会造成多个不同的对象相等但却有不同的哈希码
    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        // 比较类的定义是否一致
        if (this.getClass() != obj.getClass())
            return false;
        // 类的定义一致，则对象obj就可被强制类型转换成Student
        Student other = (Student) obj;
        return this.name.equals(other.name) && this.age == other.age;
    }

    // hashCode()方法被重写后，返回的值就不再是对象的内存地址
    @Override
    public int hashCode() {
        return this.name.hashCode() + this.age;
    }

}
```

```Java
public class StudentTest {

    public static void main(String[] args) {
        Student s1 = new Student("Phainon", 21);
        Student s2 = new Student("Phainon", 21);
        boolean res = s1.equals(s2); // true
        System.out.println(res);
        System.out.println(s1.hashCode());
        System.out.println(s2.hashCode());
    }
    
}
```

### 结论

根据定义，如果两个对象相等，则它们的哈希码也必须相等，反之则不然。  
重写了 equals 方法，就需要重写 hashCode 方法，才能满足上面的结论。  

### 面试题：请描述 == 和 equals 方法的区别

**基本数据类型**使用 == 比较的就是两个数据的**字面量**是否相等。**引用数据类型**使用 == 比较的是**内存地址**。

equals 方法来自 Object 类，本身实现使用的就是 ==，此时它们之间没有区别。但是 Object 类中的 equals 方法可能被重写，此时比较就需要看重写逻辑来进行。

## toString()

```Java
public String toString() {
		return getClass().getName() + "@" + Integer.toHexString(hashCode());
}

// 应该始终考虑在类中重写toString() 方法。
// Object的toString() 方法返回该对象的String表示形式，这对于调试非常有用。 对象的String表示形式完全取决于对象，这就是为什么
// 需要在类中重写toString() 的原因。
```

### 举例

```Java
public class Student {

    String name;
    int age;

    public Student() {}
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student [name=" + name + ", age=" + age + "]";
    }
    
}
```

```Java
package ex5;

public class StudentTest {

    public static void main(String[] args) {
        Student s1 = new Student("Phainon", 21);
        System.out.println(s1);
        // Student [name=Phainon, age=21]
    }
}
```

## finalize()

```Java
@Deprecated(since="9")
protected void finalize() throws Throwable { }

// Object类提供了一个回调方法 finalize() ，当该对象变为垃圾时可以在该对象上调用该方法。Object类的finalize()实现不执行任何操作——你可以覆盖 finalize() 进行清理，例如释放资源。
```

### 举例

```Java
public class Student extends Person{

    String name;
    int age;

    public Student() {}
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 当一个Student对象变成垃圾时可能会被调用
    @Override
    protected void finalize() throws Throwable {
        this.name = null;
        System.out.println("已释放内存");
    }

}
```

```Java
public class StudentTest {

    public static void main(String[] args) {
        show();
        // garbage collector
        System.gc(); // 调用系统的垃圾回收器进行垃圾回收
    }

    public static void show() {
        // s对象的作用范围只是在show()方法中，一旦方法执行完毕，s对象就应该消亡，释放内存
        Student s = new Student("Phainon", 21);
        System.out.println(s);
    }
}
```