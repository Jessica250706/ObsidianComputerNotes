# Inheritance/继承

## 概念

1. The idea of inheritance is simple but powerful: When you want to create a new class and there is already a class that includes some of the code that you want, you can derive your new class from the existing class. In doing this,  
    you can reuse the fields and methods of the existing class without having to  
    write (and debug!) them yourself.  
    
    继承的概念很简单但是很强大：当你要创建一个新类并且已经有一个包含所需代码的类时，可以从现有类中派生新类。这样，你可以重用现有类的字段和方法，而不必自己编写（和调试！）它们。
    
2. A subclass inherits all the members (fields, methods, and nested classes)  
    from its superclass. Constructors are not members, so they are not inherited  
    by subclasses, but the constructor of the superclass can be invoked from the  
    subclass.  
    
    子类从其父类继承所有成员（字段，方法和嵌套类）。构造方法不是成员，因此它们不会被子类继承，但是可以从子类中调用父类的构造方法。
    
3. A class that is derived from another class is called a subclass (also a  
    derived class, extended class, or child class). The class from which the  
    subclass is derived is called a superclass (also a base class or a parent  
    class).  
    
    从一个类派生的类称为子类（也可以是派生类，扩展类或子类）。派生子类的类称为超类（也称为基类或父类）。
    

p.s. 继承也是面向对象的三大特征之一。

## 语法

```Java
 public class 子类名 extends 父类名 {
 
 }
```

## 举例

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄
    
    // 方法
    public void showName()
    {
        System.out.println(name);
    }

}
```

```Java
public class Student extends Person{

    public void Show() {
        // 未在本类中定义，但能使用，是从父类中继承过来的
        System.out.println(name);
        showName();
    }

}
```

## 继承的属性和方法

1. A subclass inherits all of the **public** and **protected** members of its parent, no  
    matter what package the subclass is in. If the subclass is in the same  
    package as its parent, it also inherits the package-private members of the  
    parent.  
    
    不论子类在什么包中，子类会继承父类中所有的**公开**的和**受保护**的成员（包括字段和方法）。如果子类和父类在同一个包中，子类也会继承父类中受包保护的成员。
    
2. A subclass does not inherit the private members of its parent class. However,  
    if the superclass has public or protected methods for accessing its private  
    fields, these can also be used by the subclass.  
    
    子类不会继承父类中定义的私有成员。尽管如此，如果父类有提供公开或者受保护的访问该字段的方法，这些方法也能在子类中被使用。
    

**p.s. 若一个对象赋值给其父类的引用，此时想要调用该对象的特有的方法，必须要进行强制类型转换。**

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄

    // 方法
    public void showName()
    {
        System.out.println(name);
    }

    public void eat() {
        System.out.println("eat");
    }

}
```

```Java
public class Doctor extends Person {

    private String professional;

    public void cure() {
        System.out.println("医生治病");
    }

}
```

```Java
public class Programmer extends Person {

    private int level;

    public void programming() {
        System.out.println("Programming with Java");
    }
}
```

```Java
public final class Main {

    public static void main(String[] args) {

        Doctor d = new Doctor();
        d.cure();
        d.eat();

        Person p1 = new Doctor();
        p1.eat();
        ((Doctor)p1).cure(); // 强制类型转换

        Person p2 = new Programmer();
        p2.eat();
        ((Programmer)p2).programming(); // 强制类型转换

    }
}
```

# Override/方法重写

## 概念

An instance method in a subclass with the same signature(name, plus the number and the type of its parameters) and return type as an instance method in the superclass overrides the superclass's method.

子类中的一个成员方法与父类中的成员方法有相同的**签名**（方法名加上参数数量和参数类型）和**返回值类型**的实例方法重写了父类的方法。

## HOW

1. The ability of a subclass to override a method allows a class to inherit from a superclass whose behavior is "close enough" and then to modify behavior as needed. The overriding method has the same name, number and type of parameters, and return type as the method that it overrides. An overriding method can also return a subtype of the type returned by the overridden  
    method. This subtype is called a covariant return type.  
    
    子类重写方法的能力使类可以从**行为“足够近”**的父类继承，然后根据需要修改行为。重写方法与被重写的方法具有相同的名称，数量和参数类型，并且返回类型相同。重写方法还可以返回重写方法返回的类型的子类型。 此子类型称为**协变返回类型**。
    
    p.s. 子类重写父类方法时，返回值类型可以是父类方法的返回值类型的子类
    
2. When overriding a method, you might want to use the @Override annotation that instructs the compiler that you intend to override a method in the superclass. If, for some reason, the compiler detects that the method does not exist in one of the superclasses, then it will generate an error.
    
    重写方法时，您可能需要使用**@Override注解**，该注释指示编译器您打算重写父类中的方法。 如果由于某种原因，编译器检测到该方法在父类中不存在，则它将生成错误。
    

![[image 18.png|image 18.png]]

## 举例

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄

    // 方法
    public void showName()
    {
        System.out.println(name);
    }

    public void eat() {
        System.out.println("eat");
    }

}
```

```Java
public class Doctor extends Person {

    private String professional;

    public void cure() {
        System.out.println("医生治病");
    }

    @Override
    public void eat() {
        System.out.println("用手术刀吃饭");
    }

}
```

```Java
public class Programmer extends Person {

    private int level;

    public void programming() {
        System.out.println("Programming with Java");
    }

    @Override // 注解，告诉编译器这是一个重写的方法，编译器会检测父类中是否存在这样的方法，若不存在会报错
    public void eat() {
        System.out.println("Eating Java");
    }
}
```

p.s. 重写方法时访问修饰符的级别不能降低。

## super关键字

### 概念

If a constructor does not explicitly invoke a superclass constructor, the Java compiler automatically inserts a call to the no-argument constructor of the superclass. If the super class does not have a no-argument constructor, you will get a compile-time error. Object does have such a constructor, so if Object is the only superclass, there is no problem.

如果子类的构造方法没有明确调用父类的构造方法，Java编译器会自动插入一个父类无参构造的调用。如果父类没有无参构造，你将得到一个编译时错误。Object类有一个无参构造，因此，如果Object类是该类的唯一父类，这就没有问题。

### 举例

1. 子类和父类中都没有定义构造方法

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄

    // 方法

    public Person() {

    }

}
```

```Java
public class Doctor extends Person {

    private String professional;

    // 若一个类中没有定义构造方法，那么编译器将会给该类插入一个无参构造方法
    public Doctor() {
        super(); // 若子类的构造方法中没有显示的调用父类的构造方法，那么编译器会自动插入一个父类无参构造的调用
    }

}
```

1. 子类中有定义构造方法，父类没有定义构造方法

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄
}
```

```Java
public class Doctor extends Person {

    private String professional;

    // 若一个类中没有定义构造方法，那么编译器将会给该类插入一个无参构造方法
    public Doctor() {
        super(); // 若子类的构造方法中没有显示的调用父类的构造方法，那么编译器会自动插入一个父类无参构造的调用
    }

    public Doctor(String name, int age, String professional) {
        super();
        this.name = name;
        this.age = age;
        this.professional = professional;
    }

}
```

1. 子类和父类中都有定义构造方法

```Java
import java.lang.String;

public class Person {
    // 变量
    public String name; // 姓名
    public int age; // 年龄

    // 方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

}
```

```Java
public class Doctor extends Person {

    private String professional;

    public Doctor(String name, int age, String professional) {
        super(name, age); // 如果父类中定义了带参构造且未定义无参构造，则必须在子类的构造方法中显示调用父类的带参构造
        this.professional = professional;
    }

}
```

p.s. 使用super关键字调用父类的构造方法时，必须为这个构造方法的第一条语句。

> If your method overrides one of its superclass's methods, you can invoke the overridden method through the use of the keyword super. You can also use super to refer to a hidden field (although hiding fields is discouraged).

> 如果你的方法重写了父类的方法之一，则可以通过使用关键字super来调用父类中被重写的方法。 你也可以使用super来引用隐藏字段（尽管不建议使用隐藏字段）。

```Java
import java.lang.String;

public class Person {
    // 变量
    protected String name; // 姓名
    public int age; // 年龄

    // 方法

    public void showName()
    {
        System.out.println(name);
    }

}
```

```Java
public class Doctor extends Person {

    private String name; // 隐藏字段

    public Doctor(String name) {
        this.name = name;
    }

    // 通过重写方法，获取隐藏字段
    @Override
    public void showName() {
        System.out.println(name);
    }
    
    // 打印子类中的name和父类中的name
    public void show() {
        System.out.println(this.name); // 访问本类中定义的name变量
        System.out.println(super.name); // 访问父类中定义的name变量
        // 若子类中和父类中没有相同的成员变量，此时使用this和super均可以调用父类中的成员变量
    }

}
```

```Java
public final class Main {

    public static void main(String[] args) {

        Person p = new Doctor("Jessica");
        p.showName();
        // 若子函数中无重写函数，则输出null
        // 若子函数中有重写函数，则输出Jessica
        
        ((Doctor)p).show(); // 输出Jessica和null

    }
}
```

p.s. super关键字还可以用在方法上。

### 思考

Q：如果子类中的静态方法与父类中的静态方法具有相同的签名，是否属于方法重写？

A：不属于方法重写。因为静态方法称之为**类方法**，跟对象无关，调用时**只看对象的数据类型**。

```Java
package ex1;

public class StaticFather {

    public static void show() {
        System.out.println("这是父类的静态方法");
    }
}
```

```Java
package ex1;

public class StaticChild extends StaticFather{

    public static void show() {
        System.out.println("This is a static child");
    }
}
```

```Java
package ex1;

public class StaticTest {

    public static void main(String[] args) {
        StaticFather sf = new StaticChild();
        sf.show(); // 这是父类的静态方法
        StaticFather.show(); // 这是父类的静态方法
        StaticChild.show(); // This is a static child
    }
}
```

## 万物皆对象

### 概念

1. Excepting Object, which has no superclass, every class has one and only one  
    direct superclass (single inheritance). In the absence of any other explicit  
    superclass, every class is implicitly a subclass of Object.  
    
    除了没有父类的Object之外，每个类都有一个且只有一个直接父类**（单继承）**。在没有其他任何显式超类的情况下，每个类都隐式为Object的子类。
    
2. Classes can be derived from classes that are derived from classes that are  
    derived from classes, and so on, and ultimately derived from the topmost  
    class, Object. Such a class is said to be descended from all the classes in  
    the inheritance chain stretching back to Object.  
    
    类可以派生自另一个类，另一个类又可以派生自另一个类，依此类推，并最终派生自最顶层的类Object。据说这样的类是继承链中所有类的后代，并延伸到Object。
    

所有类都是Object的子类，因此，创建对象时都需要调用Object类中的无参构造方法，而Object本身就表示对象，因此创建出来的都是对象。

![[image 1 5.png|image 1 5.png]]

![[image 2 5.png|image 2 5.png]]

# final修饰符

## 应用范围

final 修饰符应该使用在**类**、**变量**以及**方法**上。

## final修饰类

> Note that you can also declare an entire class final. A class that is declared final cannot be subclassed. This is particularly useful, for example, when creating an immutable class like the String class.

> 注意，你也可以声明整个类的final。 声明为final的类不能被子类化。 例如，当创建不可变类（如String类）时，这特别有用。

如果一个类被final修饰，表示这个类是**最终的类**，因此这个类**不能够在被继承**，因为继承就是对类进行扩展。

### 举例

```Java
import java.lang.String;

public final class Person {
    // 变量
    protected String name; // 姓名
    public int age; // 年龄

    // 方法
    public void showName()
    {
        System.out.println(name);
    }

}
```

```Java
// 会报错
public class Doctor extends Person {

}
```

## final修饰方法

> You can declare some or all of a class's methods final. You use the final keyword in a method declaration to indicate that the method cannot be overridden by subclasses. The Object class does this—a number of its methods are final.

> 你可以将类的某些或所有方法声明为final。 在方法声明中使用final关键字表示该方法不能被子类覆盖。 Object类就是这样做的-它的许多方法都是最终的。

### 举例

```Java
import java.lang.String;

public class Person {
    // 变量
    protected String name; // 姓名
    public int age; // 年龄

    // 方法
    public final void showName()
    {
        System.out.println(name);
    }

}
```

```Java
public class Doctor extends Person {

    private String name; // 隐藏字段

    public Doctor(String name) {
        this.name = name;
    }

		// 会报错，因为父类中的showName()方式是最终的，不可被重写
    public void showName() {
        System.out.println(name);
    }

}
```

## final修饰变量

final 修饰变量的时候，变量必须在对象构建时完成初始化。final修饰的变量称为常量。

### 举例

```Java
import java.lang.String;

public class Person {
    // 变量
    protected String name; // 姓名
    // final修饰的变量一定要在对象创建时完成赋值操作，final修饰的变量称之为常量，不可被更改
    public final int age; // 年龄
    // static final修饰的变量就是静态常量
    public static final String COUNTRY = "CHINA";

    public Person(int age) {
        this.age = age; // 必须在构造函数中赋值，否则会报错
    }

    // 方法
    public void change() {
        // this.age = 21;
        // 因为age是一个常量，不能再被更改，因此会报编译错误
    }

}
```

## 其他

构建Child对象时，发现Child是Father的子类，而Father又是Object的子类。因此JVM会首先加载Object类，然后再加载Father类，最后再加载Child类。

而**静态代码块**是在类第一次加载时执行，而且只会执行一次。因此Father类中的静态代码块先执行，然后再执行Child类中的静态代码块。然后才执行newChild();代码。

### 执行顺序

1. 父类静态代码块执行
2. 子类静态代码块执行
3. 父类构造方法执行
4. 子类构造方法执行