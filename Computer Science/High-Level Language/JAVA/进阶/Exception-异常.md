# Exception/异常

## 概念

> An exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions.  
> 异常是在程序执行期间发生的事件，该事件中断了程序指令的正常流程。  
>  
> When an error occurs within a method, the method creates an object and hands it off to the runtime system. The object, called an exception object, contains information about the error, including its type and the state of the program when the error occurred. Creating an exception object and handing it to the runtime system is called throwing an exception.  
> 当方法内发生错误时，该方法将创建一个对象并将其交给运行时系统。 该对象称为异常对象，包含有关错误的信息，包括错误的类型和发生错误时程序的状态。创建异常对象并将其交给运行时系统称为**抛出异常**。

p.s. 异常是由方法抛出。

## 体系

![[image 19.png|image 19.png]]

### Throwable/可抛出的

是所有异常的父类。其常用方法如下：

```Java
package java.lang;

public Throwable();

public Throwable(String message);

public String getMessage(); // 获取异常发生的原因

public void printStackTrace(); // 打印异常在栈中的轨迹信息
```

### Error/错误

Error 是一种非常严重的错误，程序员不能通过编写解决。

### Exception/异常

Exception 表示异常的意思，主要是程序员在编写代码时考虑不周导致的问题。异常分为**运行时异常**和**检查异常**两大类，一旦程序出现这些异常，程序员应该处理这些异常。

### `RuntimeException` - 运行时异常

`RuntimeException` 表示运行时异常，所有在程序运行的时候抛出的异常类型都是属于 `RuntimeException` 的子类。运行时异常一般来说程序可以自动恢复，不必处理。

### `CheckedException` - 检查异常

检查异常是指编译器在编译代码的过程中发现不正确的编码所抛出的异常。

# 异常处理

## HOW

在 Java 中，异常的种类很多，如果每一种异常类型我们都需要去记住，这无疑是一件很困难的事。如果能够有一套机制来处理异常，那么将减少程序员在开发时的耗时。Java 就提供了一套异常处理机制来出来异常。Java 处理异常使用了 5 个关键字：throw、throws、try、catch、finally。

## throw 抛出异常

throw 关键字只能在方法内部使用，throw 关键字抛出异常表示自身并未对异常进行处理。

### 语法

```Java
throw 异常对象; //通常与if选择结构配合使用
```

### 举例

```Java
import java.util.Scanner;

public final class Main {

    public static void calculate(){
        Scanner sc = new Scanner(System.in);
        int number1 = sc.nextInt();
        int number2 = sc.nextInt();
        if (number2 == 0){
            throw new ArithmeticException("在除法运算中，除数不能为0");
        }
        int result = number1 / number2;
        System.out.println(result);
    }

    public static void main(String[] args) {
        calculate();
    }
}
```

```Java
if (number2 == 0){
		ArithmeticException e = new ArithmeticException("在除法运算中，除数不能为0");
    throw e;
}
```

## throws 声明可能抛出的异常类型

throws 关键字只能应用在**方法**或者**构造方法的定义上**对可能抛出的异常类型进行声明，自身不会对异常做出处理，由方法的调用者来处理。如果方法的调用者未处理，则异常将持续向上一级调用者抛出，直至 main() 方法为止，如果 main() 方法也未处理，那么程序可能因此终止。

### 语法

```Java
访问修饰符 返回值类型 方法名(参数列表) throws 异常类型1,异常类型2,...异常类型n{
    
}
```

### 举例

```Java
import java.util.InputMismatchException;
import java.util.Scanner;

public class ThrowsExample {

    private static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        int number = divided();
        System.out.println(number);
    }

    // 执行该方法时，可能会抛出InputMismatchException
    public static int getNumber() throws InputMismatchException {
        System.out.println("Enter a number: ");
        int number = sc.nextInt();
        return number;
    }

    public static int divided() throws InputMismatchException, ArithmeticException {
        int number1 = getNumber();
        int number2 = getNumber();
        return number1 / number2;
    }

}
```

throws 可以声明方法执行时可能抛出的异常类型，但需要注意的是：方法执行过程中只能抛出声明的异常类型的其中**一个**异常。

## try-catch 捕获异常

throw 和 throws 关键字均没有对异常进行处理，这可能会导致程序终止。在这种情况下，可以使用 try-catch 结构来对抛出异常进行捕获处理，从而保证程序能够正常运行。

### 语法

```Java
try{
    // 代码块
} catch(异常类型 异常对象名){
    
}
```

其中 try 表示尝试的意思，尝试执行 try 结构中的代码块，如果执行过程中抛出了异常，则交给 catch 语句块进行捕获操作。

### 举例

```Java
package ex6;

import java.util.InputMismatchException;
import java.util.Scanner;

public class TryCatchExample {

    private static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        try {
            int number = getNumber();
            System.out.println(number);
        } catch (InputMismatchException e) {
            e.printStackTrace(); // 打印异常轨迹
            System.out.println(e.getClass().getName());
            System.out.println("异常信息：" + e.getMessage());
            System.out.println("输入类型不匹配，请输入数字");
        }
        System.out.println("发生异常也会执行");
    }

    public static int getNumber() {
        System.out.println("Enter a number: ");
        int number = sc.nextInt();
        return number;
    }

}
```

### 思考：如果一个方法可能抛出多个异常，如何捕获？

可以在 try 后面添加多个 catch 子句来分别对每一种异常进行处理。

```Java
import java.util.InputMismatchException;
import java.util.Scanner;

public class TryCatchExample {

    private static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        int res = divided();
        System.out.println(res);
    }

    public static int getNumber() {
        System.out.println("Enter a number: ");
        int number = sc.nextInt();
        return number;
    }

    public static int divided() {
        try {
            int number1 = getNumber();
            int number2 = getNumber();
            return number1 / number2;
        } catch (InputMismatchException e) {
            System.out.println("输入类型不匹配，请输入数字");
        } catch (ArithmeticException e) {
            System.out.println("除数不能为0");
        }
        return 0;
    }

}
```

p.s. 当使用多个 catch 子句捕获异常时，如果捕获的多个异常对象的数据类型具有**继承关系**，那么父类异常不能放在前面。

```Java
public static int divided() {
    try {
        int number1 = getNumber();
        int number2 = getNumber();
        return number1 / number2;
    } catch (Exception e) { // 报错
        System.out.println("输入类型不匹配，请输入数字");
    } catch (ArithmeticException e) {
        System.out.println("除数不能为0");
    }
    return 0;
}
```

## finally 语句

finally 语句不能单独使用，必须与 try 语句或者 try-catch 结构配合使用，表示**无论程序是否发生异常都会执行**，主要用于释放资源。但**如果在 try 语句或者 catch 语句中存在系统退出的代码，则 finally 语句将得不到执行**。

```Java
System.exit(0); //系统正常退出 0-正常退出 非0-异常退出
System.exit(1); //系统异常退出
```

### 语法

```Java
try{
    
} finally{
    
}

// 或者

try{
    
} catch(异常类型 异常对象名){
    
} finally{
    
}
```

### 举例

```Java
import java.util.Scanner;

public class FinallyExample {

    private Scanner sc = new Scanner(System.in);
    private static int[] numbers = {1, 2, 3, 4, 5};

    public static void main(String[] args) {
        try {
            int number = getNumberFromArray(5);
            System.out.println(number);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组下标越界");
        }finally {
            System.out.println("需要执行的代码");
        }
    }

    public static int getNumberFromArray(int index) {
        return numbers[index];
    }

}
```

### 面试题

```Java
public class Test {

    public static void main(String[] args) {
        int res = getResult();
        System.out.println(res); // 10
    }

    public static int getResult() {
        int number = 10;
        try { // 尝试执行
            return number;
        } catch (Exception e) {
            return 1;
        } finally {
            number++;
        }
    }
    
}
```

解释：返回值 ⇒ 尝试返回一个结果，但发现后面还有 finally 模块，而 finally 模块一定会得到执行。于是在这里只能将返回值使用一个临时变量存储起来。然后再执行 finally 模块，finally 模块执行完之后，再将这个临时变量存储的值返回

# 自定义异常

## WHY

在 Java 中，异常的类型非常的多，要想使用这些异常，首先必须要熟悉它们。这无疑是一个巨大的工作量，很耗费时间。如果我们可以自定异常，则只需要熟悉 RuntimeException 、Exception 和 Throwable 即可。这大大缩小了熟悉范围。自定义异常还可以帮助我们快速的定位问题。

## 语法

### 自定义运行时异常语法

```Java
 public class 类名 extends RuntimeException{}
```

### 自定义检查异常语法

```Java
public class 类名 extends Exception{}
```

### 举例

在登录时经常会看到提示：" 用户名不存在 " 或者 " 账号或密码错误 "。请使用自定义异常来描述该场景：

```Java
/**
 * 用户名不存在-异常
 *
 * 异常命名规范：场景描述+Exception
 */

public class UsernameNotFoundException extends Exception{

    public UsernameNotFoundException(){}
    public UsernameNotFoundException(String msg){
        super(msg);
    }
    
}
```

```Java
/**
 * 账号或密码错误异常
 */

public class BadCredentialsException extends Exception{

    public BadCredentialsException() {}
    public BadCredentialsException(String message) {
        super(message);
    }

}
```

```Java
import java.util.Scanner;

public class Login {

    private static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) throws UsernameNotFoundException {
        System.out.println("Enter your username: ");
        String username = sc.next();
        System.out.println("Enter your password: ");
        String password = sc.next();

        try {
            login(username, password);
        } catch (UsernameNotFoundException e) {
            e.printStackTrace();
        } catch (BadCredentialsException e) {
            e.printStackTrace();
        }
    }

    public static void login(String username, String password) throws UsernameNotFoundException, BadCredentialsException {
        if ("admin".equals(username)) {
            if ("123456".equals(password)) {
                System.out.println("You have successfully logged in!");
            } else {
                throw new BadCredentialsException("账号或密码错误");
            }
        } else {
            throw new UsernameNotFoundException("账号不存在");
        }
    }
}
```

## 注意事项

1. 运行时异常可以不处理。
2. 如果父类抛出了多个异常,子类覆盖父类方法时，只能抛出相同的异常或者是该异常的子集。(与协变返回类型原理一致)
3. 父类方法没有抛出异常，子类覆盖父类该方法时也不可抛出检查异常。此时子类产生该异常，只能捕获处理，不能声明抛出。