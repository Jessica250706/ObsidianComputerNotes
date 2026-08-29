---
tags:
  - 数据库
  - JDBC
---

# JDBC

## 概念

JDBC (Java Database Connection) 是 Java 数据库连接技术的简称，提供连接数据库的能力。

## JDBC API

Java 作为目前世界上最流行的高级开发语言，当然不可能考虑去实现各种数据库的连接与操作。但 Java 语言的开发者对数据库的连接与操作提供了相关的接口，供各大数据库厂商去实现。这些接口位于 java.sql 包中。

### Driver

java.sql.Driver ：数据库厂商提供的 JDBC 驱动包中必须包含该接口的实现，该接口中就包含连接数据库的功能。

```Java
// 根据给定的数据库url地址连接数据库
Connection connect(String url, java.util.Properties info) throws SQLException;
```

### DriverManager

java.sql.DriverManager ：数据库厂商的提供的 JDBC 驱动交给 DriverManager 来管理，DriverManager 主要负责获取数据库连接对象 Connection。

```Java
// 通过给定的账号、密码和数据库地址获取一个连接
public static Connection getConnection(String url, String user, String password) throws SQLException
```

### Connection

java.sql.Connection ：连接接口，数据库厂商提供的 JDBC 驱动包中必须包含该接口的实现，该接口主要提供与数据库的交互功能。

```Java
// 创建一个SQL语句执行对象
Statement createStatement() throws SQLException;

// 创建一个预处理SQL语句执行对象
PreparedStatement prepareStatement(String sql) throws SQLException;

// 创建一个存储过程SQL语句执行对象
CallableStatement prepareCall(String sql) throws SQLException;

// 设置该连接上的所有操作是否执行自动提交
void setAutoCommit(boolean autoCommit) throws SQLException;

// 提交该连接上至上次提交以来所作出的所有更改
void commit() throws SQLException;

// 回滚事务，数据库回滚到原来的状态
void rollback() throws SQLException;

// 关闭连接
void close() throws SQLException;

// 设置事务隔离级别
void setTransactionIsolation(int level) throws SQLException;
```

```Java
// 不支持事务
int TRANSACTION_NONE             = 0;

// 读取未提交的数据
int TRANSACTION_READ_UNCOMMITTED = 1;

// 读取已提交的数据
int TRANSACTION_READ_COMMITTED   = 2;

// 可重复读
int TRANSACTION_REPEATABLE_READ  = 4;

// 串行化
int TRANSACTION_SERIALIZABLE     = 8;
```

![[image 35.png|image 35.png]]

### Statement

java.sql.Statement ：SQL 语句执行接口，数据库厂商提供的 JDBC 驱动包中必须包含该接口的实现，该接口主要提供执行 SQL 语句的功能。

```Java
// 执行查询，得到一个结果集
ResultSet executeQuery(String sql) throws SQLException;

// 执行更新，得到受影响的行数
int executeUpdate(String sql) throws SQLException;

// 关闭SQL语句执行器
void close() throws SQLException;

// 将SQL语句添加到批处理执行SQL列表中
void addBatch( String sql ) throws SQLException;

// 执行批处理，返回列表中每一条SQL语句的执行结果
int[] executeBatch() throws SQLException;
```

### ResultSet

java.sql.ResultSet ：查询结果集接口，数据库厂商提供的 JDBC 驱动包中必须包含该接口的实现，该接口主要提供查询结果的获取功能。

```Java
// 光标从当前位置（默认位置位为0）向前移动一行，如果存在数据，则返回true，否则返回false
boolean next() throws SQLException;

// 关闭结果集
void close() throws SQLException;

// 获取指定列的字符串值
String getString(int columnIndex) throws SQLException;

// 获取指定列的布尔值
boolean getBoolean(int columnIndex) throws SQLException;

// 获取指定列的整数值
int getInt(Sting columnName) throws SQLException;

// 获取指定列的对象
Object getObject(int columnIndex, Class type) throws SQLException;

// 获取结果集元数据：查询结果的列名称、列数量、列别名等等
ResultSetMetaData getMetaData() throws SQLException;

// 光标从当前位置（默认位置位为0）向后移动一行，如果存在数据，则返回true，否则返回false
boolean previous() throws SQLException;
```

## JDCB 操作步骤

### 引入驱动包

新建工程后，将 mysql-connector-java.jar 引入工程中。

![[Pasted image 20250502172521.png]]

记得勾选。

![[Pasted image 20250502190302.png]]

![[image 1 14.png|image 1 14.png]]

### 加载驱动

```Java
// MySQL 5.0
// Class.forName("com.mysql.jdbc.Driver");
// MySQL 8.0
Class.forName("com.mysql.cj.jdbc.Driver");
```

### 获取连接：从驱动管理器中获取连接

```Java
Connection connection = DriverManager.getConnection(url, username, password);
```

### 创建 SQL 语句执行器

```Java
Statement statement = connection.createStatement();
```

### 执行 SQL 语句

```Java
// 查询
ResultSet rs = statement.executeQuery(sql);
while(rs.next()){
		// 获取列信息
}

// 更新
int affectedRows = statement.executeUpdate();
```

### 释放资源

```Java
rs.close();
statement.close();
connection.close();
```

### 完整代码 - 示例

```Java
package com.cyx.jdbc;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class JdbcTest {

	public static void main(String[] args) {
		// jdbc:使用jdbc连接技术
		// mysql://localhost:3306 使用的是MySQL数据库协议，访问的是本地计算机3306端口
		String url = "jdbc:mysql://localhost:3306/lesson?serverTimezone=Asia/Shanghai";
		String username = "root";
		String password = "root";
		List<Account> accounts = new ArrayList<>();
		// MySQL8.0
		try {
				// 加载驱动
				Class.forName("com.mysql.cj.jdbc.Driver");
				// 获取连接
				Connection conn = DriverManager.getConnection(url, username, password);
				// 在连接上创建SQL语句执行器
				Statement s = conn.createStatement();          
				String sql = "SELECT account,balance,state FROM account";         
				// 使用执行器执行查询，得到一个结果集
				ResultSet rs = s.executeQuery(sql);
				while (rs.next()){//光标移动
						// 通过列名称获取列的值
						String account = rs.getString("account");
						double balance = rs.getDouble(2);
						int state = rs.getInt("state");
						Account a = new Account(account, balance, state);
						accounts.add(a);
				}
				rs.close();
				String updateSql = "UPDATE account SET balance = balance + 1000 WHERE account=123457";
				// 执行更新时，返回的都是受影响的行数
				int affectedRows = s.executeUpdate(updateSql);
				System.out.println(affectedRows);
				s.close();
				conn.close();
		} catch (ClassNotFoundException e) {
				e.printStackTrace();
		} catch (SQLException throwables) {
				throwables.printStackTrace();
		}
		accounts.forEach(System.out::println);
	}
		
}
```

### 完整代码 -MINE（注意密码泄露）

```Java
package com.kswl.jdbc;

public class Account {

    private String account;
    private double balance;
    private int state;

    public Account(String account, double balance, int state) {
        this.account = account;
        this.balance = balance;
        this.state = state;
    }
    public Account() {}

    public String getAccount() {
        return account;
    }
    public void setAccount(String account) {
        this.account = account;
    }

    public double getBalance() {
        return balance;
    }
    public void setBalance(double balance) {
        this.balance = balance;
    }

    public int getState() {
        return state;
    }
    public void setState(int state) {
        this.state = state;
    }

    @Override
    public String toString() {
        return "Account [account=" + account + ", balance=" + balance + ", state=" + state + "]";
    }
}
```

```Java
package com.kswl.jdbc;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class jdbcTest {

    public static void main(String[] args) {
        // jdbc: 使用JDBC连接技术
        // mysql://localhost:3306 使用的是MySQL数据库协议，访问的是本地计算机3306端口
        // 类比http://www.daibu.com
        // 后接数据库名称
        // MySQL 8.0需要加上?serverTimezone=Asia/Shanghai
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";
        String username = "root";
        String password = "Fxly2020c&y";
        // 定义集合
        List<Account> accounts = new ArrayList<Account>();
        try {
            // 加载驱动
            // MySQL 5.0
            // Class.forName("com.mysql.jdbc.Driver");
            // MySQL 8.0
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 获取连接
            Connection con = DriverManager.getConnection(url, username, password);
            // 创建SQL语句执行器
            Statement s = con.createStatement();
            // 执行SQL语句， 获得一个结果集
            ResultSet rs = s.executeQuery("SELECT * FROM account");
            // 重复：光标向下移动
            while (rs.next()) {
                // 通过各列名称获取列的值
                String account = rs.getString("account");
                double balance = rs.getDouble("balance");
                int state = rs.getInt("state");
                Account ac = new Account(account, balance, state);
                accounts.add(ac);
            }
            // 释放资源
            rs.close();
            s.close();
            con.close();
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
        // 打印
        accounts.forEach(System.out::println);
    }

}
```

- 更新

```Java
package com.kswl.jdbc;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class jdbcTest {

    public static void main(String[] args) {
        // jdbc: 使用JDBC连接技术
        // mysql://localhost:3306 使用的是MySQL数据库协议，访问的是本地计算机3306端口
        // 类比http://www.daibu.com
        // 后接数据库名称
        // MySQL 8.0需要加上?serverTimezone=Asia/Shanghai
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";
        String username = "root";
        String password = "Fxly2020c&y";
        // 定义集合
        List<Account> accounts = new ArrayList<Account>();
        try {
            // 加载驱动
            // MySQL 5.0
            // Class.forName("com.mysql.jdbc.Driver");
            // MySQL 8.0
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 获取连接
            Connection con = DriverManager.getConnection(url, username, password);
            // 创建SQL语句执行器
            Statement s = con.createStatement();
            // 更新语句
            String updateSQL = "UPDATE account SET balance = balance + 1000 WHERE account = 123457";
            // 执行更新时，返回的都是受影响的行数
            int affectedRows = s.executeUpdate(updateSQL);
            // 打印
            System.out.println("Affected rows: " + affectedRows);
            // 释放资源
            s.close();
            con.close();
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
        // 打印
        accounts.forEach(System.out::println);
    }

}
```

![[image 2 13.png|image 2 13.png]]

## 预处理 SQL

在日常开发中，我们经常会根据用户输入的信息从数据库中进行数据筛选，现有 stu 表数据如下：

![[image 3 10.png|image 3 10.png]]

现要根据用户输入的学生姓名查询学生信息。

```Java
Scanner sc = new Scanner(System.in);
System.out.println("请输入学生姓名：");
String name = sc.next();
String sql = "SELECT id, name, sex, age FROM stu WHERE name='" + name + "'";
```

如果此时用户输入信息为：张华 ' or 1='1， 那么，上面的代码执行后 SQL 语句变为：

```SQL
SELECT id, name, sex, age FROM stu WHERE name='张华' or 1='1'
```

明显查询的结果发生了变化，这样的情况被称作为**SQL 注入**。为了防止 SQL 注入，Java 提供了 PreparedStatement 接口对 SQL 进行预处理，该接口是 Statement 接口的子接口，其常用方法如下：

```Java
// 执行查询，得到一个结果集
ResultSet executeQuery() throws SQLException;

// 执行更新，得到受影响的行数
int executeUpdate() throws SQLException;

// 使用给定的整数值设置给定位置的参数
void setInt(int parameterIndex, int x) throws SQLException;

// 使用给定的长整数值设置给定位置的参数
void setLong(int parameterIndex, long x) throws SQLException;

// 使用给定的双精度浮点数值设置给定位置的参数
void setDouble(int parameterIndex, double x) throws SQLException;

// 使用给定的字符串值设置给定位置的参数
void setString(int parameterIndex, String x) throws SQLException;

// 使用给定的对象设置给定位置的参数
void setObject(int parameterIndex, Object x) throws SQLException;

// 获取结果集元数据
ResultSetMetaData getMetaData() throws SQLException;
```

如何获取 PreparedStatement 接口对象呢？

```Java
PreparedStatement ps = connection.prepareStatement(sql);
```

PreparedStatement 是如何进行预处理的？

使用 PreparedStatement 时，SQL 语句中的参数一律使用?号来进行占位，然后通过调用 setXxx() 方法来对占位的?号进行替换。从而将参数作为一个整体进行查询。

上面的示例使用 PreparedStatement 编写 SQL 语句为：

```Java
Scanner sc = new Scanner(System.in);
System.out.println("请输入学生姓名：");
String name = sc.next();
String sql = "SELECT id, name, sex, age FROM stu WHERE name=?";
PreparedStatement ps = connection.prepareStatement(sql);
ps.setString(1, name);
ResultSet rs = ps.executeQuery();
```

### 举例

```Java
import java.sql.*;
import java.util.Scanner;

public class PrepareStatemetTest {

    // 预处理
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter goods name: ");
        String goodsName = sc.nextLine();
        // 连接数据库
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";
        String username = "root";
        String password = "Fxly2020c&y";
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, username, password);
            String sql = "SELECT id,`name`,number, price, agent_id FROM goods WHERE `name`=? LIMIT 0, 20";
            // 创建预处理执行器
            PreparedStatement ps = con.prepareStatement(sql);
            // 设置占位符替换的值
            ps.setString(1, goodsName);
            ResultSet rs = ps.executeQuery();
            while (rs.next()) {
                long id = rs.getLong("id");
                String name = rs.getString("name");
                int number = rs.getInt("number");
                double price = rs.getDouble("price");
                long agentId = rs.getLong("agent_id");
                System.out.println(id + " " + name + " " + number + " " + price + " " + agentId);
            }
            rs.close();
            ps.close();
            con.close();
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
    }

	// 非预处理
    private static void sqlInjection() {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter goods name: ");
        String goodsName = sc.nextLine();
        goodsName = "'%" + goodsName + "%'";
        // 连接数据库
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";
        String username = "root";
        String password = "Fxly2020c&y";
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, username, password);
            Statement s = con.createStatement();
            String sql = "SELECT id,`name`,number, price, agent_id FROM goods WHERE `name` LIKE " + goodsName + " LIMIT 0, 20";
            ResultSet rs = s.executeQuery(sql);
            while (rs.next()) {
                long id = rs.getLong("id");
                String name = rs.getString("name");
                int number = rs.getInt("number");
                double price = rs.getDouble("price");
                long agentId = rs.getLong("agent_id");
                System.out.println(id + " " + name + " " + number + " " + price + " " + agentId);
            }
            rs.close();
            s.close();
            con.close();
        } catch (ClassNotFoundException | SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

# 反射

## Class 类

我们编写的 Java 程序先经过编译器编译，生成 class 文件，而 class 文件的执行场所是在 JVM 如何存储我们编写的类的信息？

一个类的组成部分。

![[image 4 8.png|image 4 8.png]]

如果要定义一个类来描述所有类的共有特征，应该如何来设计？

```Java
public class Class {

		private String name; // 类名
		
		private Package pk; // 包名
		
		private Constructor[] constructors; // 构造方法，因为可能存在多个，所以使用数组
		
		private Field[] fields; // 字段，因为可能存在多个，所以使用数组
		
		private Method[] methods; // 方法，因为可能存在多个，所以使用数组
		
		private Class<?> interfaces; // 实现的接口，因为可能存在多个，所以使用数组
		
		private Class<?> superClass; // 继承的父类
		
		// 省略getter/setter
		
}
```

为什么要设计这样的类？因为我们编写的程序从本质上来说也是文件，JVM 加载类的过程相当于对文件内容进行解析，解析内容就需要找到共有特征（ Class 类定义），然后再将这特征（使用 Class 对象）存储起来，在使用的时候再取出来。通过 Class 对象反向推到我们编写的类的内容，然后再进行操作，这个过程就称为**反射**。

在 JDK 中已经提供了这样的类：java.lang.Class ， 因此，我们不需要再来设计，只需要学习它即可。

### 定义

如何获取一个类对应的 Class 对象呢？

```Java
Class<类名> clazz = 类名.class;
Class<类名> clazz = 对象名.getClass();
Class<类名> clazz = clazz.getSuperClass();
Class clazz = Class.forName("类的全限定名"); // 类的全限定名=包名 + "." + 类名
Class clazz = 包装类.TYPE;
```

```Java
package com.kswl.jdbc.reflection;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        Class<Student> c1 = Student.class;  
        System.out.println(c1.getName());  
  
        Student stu = new Student("Jessica", 25);  
        Class<? extends Student> c2 = stu.getClass(); // 泛型  
        System.out.println(c2.getName());  
  
        // 获取父类  
        Class<? super Student> c3 = c1.getSuperclass();  
        System.out.println(c3.getName());  
  
        try {  
            Class c4 = Class.forName("com.kswl.jdbc.reflection.Student");  
            System.out.println(c4.getName());  
        } catch (ClassNotFoundException e) {  
            throw new RuntimeException(e);  
        }  
  
        // 包装类  
        Class c5 = Integer.TYPE;  
        System.out.println(c5.getName());  
  
    }  
}
```

```
// 输出结果
com.kswl.jdbc.reflection.Student
com.kswl.jdbc.reflection.Student
java.lang.Object
com.kswl.jdbc.reflection.Student
int
```

### Class 类常用方法

```Java
// Field字段

// 获取类中使用public修饰的字段
public Field[] getFields() throws SecurityException;

// 获取类中定义的所有字段
public Field[] getDeclaredFields() throws SecurityException;

// 通过给定的字段名获取类中定义的字段
public Field getField(String name) throws NoSuchFieldException, SecurityException;

// Method方法

// 获取类中使用public修饰的方法
public Method[] getMethods() throws SecurityException;

// 获取类中定义的所有方法
public Method[] getDeclaredMethods() throws SecurityException;

// 通过给定的方法名和参数列表类型获取类中定义的方法
public Method getDeclaredMethod(String name, Class<?>... parameterTypes) throws NoSuchMethodException, SecurityException;

// Constructor构造方法

// 获取类中使用public修饰的构造方法
public Constructor<?>[] getConstructors() throws SecurityException;

// 通过给定的参数列表类型获取类中定义的构造方法
public Constructor<T> getConstructor(Class<?>... parameterTypes) throws NoSuchMethodException, SecurityException;

// 其他

// 获取类的全限定名
public String getName();

// 获取类所在的包
public Package getPackage();

// 判断该类是否是基本数据类型
public native boolean isPrimitive();

// 判断该类是否是接口
public native boolean isInterface();

// 判断该类是否是数组
public native boolean isArray();

// 通过类的无参构造创建一个实例
public T newInstance() throws InstantiationException, IllegalAccessException;
```

```Java
import java.lang.reflect.AccessibleObject
// 修改访问权限
public void setAccessible(boolean flag) throws SecurityException；
```

### 举例

#### 1. 获取构造方法

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.util.Arrays;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        Class<Student> clazz = Student.class;  
  
        // 获取在类中定义的构造方法  
        Constructor[] constructors = clazz.getDeclaredConstructors();  
        for (Constructor c : constructors) {  
            // 访问修饰符  
            // 1-公开；2-私有  
            System.out.println(c.getModifiers());  
            // 构造方法的名字  
            String name = c.getName();  
            System.out.println(name);  
            // 参数类型  
            Class[] types = c.getParameterTypes();  
            System.out.println(Arrays.toString(types));  
        }  	
        System.out.println("********************************************");  
        // 获取类中使用public修饰的构造方法  
        constructors = clazz.getConstructors();  
        for (Constructor c : constructors) {  
            // 访问修饰符  
            // 1-公开；2-私有  
            System.out.println(c.getModifiers());  
            // 构造方法的名字  
            String name = c.getName();  
            System.out.println(name);  
            // 参数类型  
            Class[] types = c.getParameterTypes();  
            System.out.println(Arrays.toString(types));  
        }  
        System.out.println("********************************************");
        // 通过给定的参数列表类型获取类中定义的构造方法  
        try {  
            Constructor c = clazz.getConstructor(String.class, int.class);  
            // 访问修饰符  
            // 1-公开；2-私有  
            System.out.println(c.getModifiers());  
            // 构造方法的名字  
            String name = c.getName();  
            System.out.println(name);  
            // 参数类型  
            Class[] types = c.getParameterTypes();  
            System.out.println(Arrays.toString(types));  
        } catch (NoSuchMethodException e) {  
            throw new RuntimeException(e);  
        }  
  
    }  
}
```

```
// 输出结果
2
com.kswl.jdbc.reflection.Student
[]
1
com.kswl.jdbc.reflection.Student
[class java.lang.String, int]
********************************************
1
com.kswl.jdbc.reflection.Student
[class java.lang.String, int]
********************************************
1
com.kswl.jdbc.reflection.Student
[class java.lang.String, int]
```

#### 2. 获取字段

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.util.Arrays;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        Class<Student> clazz = Student.class;  
  
        // 获取类中定义的所有字段  
        Field[] fields = clazz.getDeclaredFields();  
        for (Field f : fields) {  
            System.out.print(f.getModifiers() + " ");  
            System.out.print(f.getType().getName() + " ");  
            System.out.println(f.getName());  
        }  
        System.out.println("********************************************");  
        // 获取类中使用public修饰的字段  
        fields = clazz.getFields();  
        for (Field f : fields) {  
            System.out.print(f.getModifiers() + " ");  
            System.out.print(f.getType().getName() + " ");  
            System.out.println(f.getName());  
        }  
        System.out.println("********************************************");  
        // 通过给定的字段名获取类中定义的字段  
        try {  
            Field f = clazz.getDeclaredField("name");  
            System.out.print(f.getModifiers() + " ");  
            System.out.print(f.getType().getName() + " ");  
            System.out.println(f.getName());  
        } catch (NoSuchFieldException e) {  
            throw new RuntimeException(e);  
        }  
  
    }
}
```

```
// 输出结果
2 java.lang.String name
2 int age
********************************************
********************************************
2 java.lang.String name
```

#### 3. 获取方法

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.Method;  
import java.util.Arrays;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        Class<Student> clazz = Student.class;  
  
        // 获取类中定义的所有方法（该类中定义的）  
        Method[] methods = clazz.getDeclaredMethods();  
        for (Method method : methods) {  
            System.out.print(method.getModifiers() + " ");  
            System.out.print(method.getName() + " ( ");  
            Class[] types = method.getParameterTypes();  
            for (Class type : types) {  
                System.out.print(type.getName() + ",");  
            }  
            System.out.println(" )");  
        }  
        System.out.println("********************************************");  
        // 获取类中使用public修饰的方法（包括继承的）  
        methods = clazz.getMethods();  
        for (Method method : methods) {  
            System.out.print(method.getModifiers() + " ");  
            System.out.print(method.getName() + " ( ");  
            Class[] types = method.getParameterTypes();  
            for (Class type : types) {  
                System.out.print(type.getName() + ",");  
            }  
            System.out.println(" )");  
        }  
        System.out.println("********************************************");  
        // 通过给定的方法名获取类中定义的方法  
        try {  
            Method m = clazz.getDeclaredMethod("setName", String.class);  
            System.out.print(m.getModifiers() + " ");  
            System.out.print(m.getName() + " ( ");  
            Class[] types = m.getParameterTypes();  
            for (Class type : types) {  
                System.out.print(type.getName() + ",");  
            }  
            System.out.println(" )");  
        } catch (NoSuchMethodException e) {  
            throw new RuntimeException(e);  
        }  
  
    }  
}
```

```
// 输出结果
1 getName (  )
1 toString (  )
1 setName ( java.lang.String, )
1 getAge (  )
1 setAge ( int, )
********************************************
1 getName (  )
1 toString (  )
1 setName ( java.lang.String, )
1 getAge (  )
1 setAge ( int, )
17 wait ( long,int, )
17 wait (  )
273 wait ( long, )
1 equals ( java.lang.Object, )
257 hashCode (  )
273 getClass (  )
273 notify (  )
273 notifyAll (  )
********************************************
1 setName ( java.lang.String, )
```

#### 4. 反射：构建一个学生对象，并为每个字段赋值

##### 4.1 简洁

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.Method;  
import java.util.Arrays;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        // 构建一个学生对象，并为每个字段赋值  
        Class<Student> clazz = Student.class;  
        try {  
            Constructor<? extends Student> c = clazz.getDeclaredConstructor();  
            // Student类中的无参构造方法是私有的  
            // 因此需要先修改访问权限  
            c.setAccessible(true);  
            Student s = c.newInstance();  
            // 给指定对象中的name字段赋值  
            Field namefield = clazz.getDeclaredField("name");  
            namefield.setAccessible(true); // 修改访问权限  
            namefield.set(s, "Wang"); // 赋值  
            // 给指定对象中的age字段赋值  
            Field agefield = clazz.getDeclaredField("age");  
            agefield.setAccessible(true);  
            agefield.set(s, 25);  
            // 输出  
            System.out.println(s);  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
  
    }
}
```

```
// 输出结果
Student [name=Wang, age=25]
```

##### 4.2 改进

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.Method;  
import java.util.Arrays;  
  
public class ReflectionTest {  
    public static void main(String[] args) {  
  
        // 构建一个学生对象，并为每个字段赋值  
        Class<Student> clazz = Student.class;  
        try {  
            // 获取Student类的无参构造方法  
            Constructor<? extends Student> c = clazz.getDeclaredConstructor();  
  
            // Student类中的无参构造方法是私有的  
            // 因此需要先修改访问权限，设置为可访问  
            c.setAccessible(true);  
  
            // 使用无参构造方法创建Student实例  
            Student s = c.newInstance();  
  
            // 给指定对象中的name字段赋值  
            Field namefield = clazz.getDeclaredField("name");  
            namefield.setAccessible(true); // 修改访问权限  
            namefield.set(s, "Wang"); // 赋值  
  
            // 给指定对象中的age字段赋值  
            Field agefield = clazz.getDeclaredField("age");  
            agefield.setAccessible(true); // 修改访问权限  
            agefield.set(s, 25); // 赋值  
  
            // get name => get + N + ame
            // 通过反射调用getter方法获取name值  
            // 构建getter方法名：get + Name (首字母大写)  
            // 例如：字段名是"name"，则getter方法名是"getName"  
            String fieldname = namefield.getName(); // 获取字段名"name"  
            String methodName = "get" +                        // 以"get"开头  
                    fieldname.substring(0, 1).toUpperCase() +  // 首字母大写  
                    fieldname.substring(1);          // 剩余部分  
            // 获取Student类中名为methodName的方法(无参数)  
            Method m = clazz.getDeclaredMethod(methodName);  
            // 在对象s上调用该方法，获取返回值(即name值)  
            String name = (String) m.invoke(s);  
            // 输出  
            System.out.println(name);  
  
            // set name => set + N + ame
            // 使用反射动态调用对象的setter方法修改字段值  
            // 构建setter方法名：格式为 "set" + 字段名首字母大写 + 剩余部分  
            // 例如：字段名是"name"，则setter方法名为"setName"  
            methodName = "set" +                               // 方法名前缀"set"  
                    fieldname.substring(0, 1).toUpperCase() +  // 字段首字母大写  
                    fieldname.substring(1);          // 字段名的剩余部分  
            // 通过反射获取Student类中指定的方法  
            // - methodName: 要获取的方法名(上面构建的setter方法名)  
            // - namefield.getType(): 方法的参数类型(即name字段的类型)  
            m = clazz.getDeclaredMethod(methodName, namefield.getType());  
            // 调用获取到的setter方法，为对象s的name字段设置新值"Fang"  
            // - s: 要调用方法的对象实例  
            // - "Fang": 要设置的参数值  
            m.invoke(s, "Fang");  
            // 输出  
            System.out.println(s);  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
  
    }
}
```

```
// 输出结果
Wang
Student [name=Fang, age=25]
```

## 反射与数据库

数据库查询出的每一条数据基本上都会封装为一个对象，数据库中的每一个字段值都会存储在对象相应的属性中。如果查询结果的每一个字段都与对象中的属性名保持一致，那么就可以使用反射来完成万能查询。

JdbcUtil 构建演示。

### 举例

#### 1. 查询 Agent

```Java
package com.kswl.jdbc.reflection;  
  
import java.sql.Connection;  
import java.sql.DriverManager;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.util.ArrayList;  
import java.util.List;  
  
public class JdbcUtil {  
  
    public static void main(String[] args) {  
  
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";  
        String username = "root";  
        String password = "Fxly2020c&y";  
  
        List<Agent> agents = new ArrayList<Agent>();  
  
        // 加载驱动  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
            Connection conn = DriverManager.getConnection(url, username, password);  
            String sql = "SELECT id,name,region_id FROM agent WHERE name LIKE ?";  
            PreparedStatement ps = conn.prepareStatement(sql);  
            ps.setString(1, "%魅%");  
            ResultSet rs = ps.executeQuery();  
            while (rs.next()) {  
                Agent a = new Agent();  
                a.setId(rs.getLong("id"));  
                a.setName(rs.getString("name"));  
                a.setRegionId(rs.getInt("region_id"));  
                agents.add(a);  
            }  
            rs.close();  
            ps.close();  
            conn.close();  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
  
        // 输出  
        agents.forEach(System.out::println);  
    }  
  
}
```

```
// 输出结果
Agent{id=41, name='魅族代理商00', regionId=1}
Agent{id=42, name='魅族代理商01', regionId=1}
Agent{id=43, name='魅族代理商02', regionId=1}
Agent{id=44, name='魅族代理商03', regionId=1}
Agent{id=45, name='魅族代理商04', regionId=1}
Agent{id=46, name='魅族代理商05', regionId=1}
Agent{id=47, name='魅族代理商06', regionId=1}
Agent{id=48, name='魅族代理商07', regionId=1}
Agent{id=49, name='魅族代理商08', regionId=1}
Agent{id=50, name='魅族代理商09', regionId=1}
Agent{id=111, name='魅族代理商10', regionId=2}
Agent{id=112, name='魅族代理商11', regionId=2}
Agent{id=113, name='魅族代理商12', regionId=2}
Agent{id=114, name='魅族代理商13', regionId=2}
Agent{id=115, name='魅族代理商14', regionId=2}
Agent{id=116, name='魅族代理商15', regionId=2}
Agent{id=117, name='魅族代理商16', regionId=2}
Agent{id=118, name='魅族代理商17', regionId=2}
Agent{id=119, name='魅族代理商18', regionId=2}
Agent{id=120, name='魅族代理商19', regionId=2}
Agent{id=181, name='魅族代理商20', regionId=3}
Agent{id=182, name='魅族代理商21', regionId=3}
Agent{id=183, name='魅族代理商22', regionId=3}
Agent{id=184, name='魅族代理商23', regionId=3}
Agent{id=185, name='魅族代理商24', regionId=3}
Agent{id=186, name='魅族代理商25', regionId=3}
Agent{id=187, name='魅族代理商26', regionId=3}
Agent{id=188, name='魅族代理商27', regionId=3}
Agent{id=189, name='魅族代理商28', regionId=3}
Agent{id=190, name='魅族代理商29', regionId=3}
Agent{id=251, name='魅族代理商30', regionId=4}
Agent{id=252, name='魅族代理商31', regionId=4}
Agent{id=253, name='魅族代理商32', regionId=4}
Agent{id=254, name='魅族代理商33', regionId=4}
Agent{id=255, name='魅族代理商34', regionId=4}
Agent{id=256, name='魅族代理商35', regionId=4}
Agent{id=257, name='魅族代理商36', regionId=4}
Agent{id=258, name='魅族代理商37', regionId=4}
Agent{id=259, name='魅族代理商38', regionId=4}
Agent{id=260, name='魅族代理商39', regionId=4}
Agent{id=321, name='魅族代理商40', regionId=5}
Agent{id=322, name='魅族代理商41', regionId=5}
Agent{id=323, name='魅族代理商42', regionId=5}
Agent{id=324, name='魅族代理商43', regionId=5}
Agent{id=325, name='魅族代理商44', regionId=5}
Agent{id=326, name='魅族代理商45', regionId=5}
Agent{id=327, name='魅族代理商46', regionId=5}
Agent{id=328, name='魅族代理商47', regionId=5}
Agent{id=329, name='魅族代理商48', regionId=5}
Agent{id=330, name='魅族代理商49', regionId=5}
```

#### 2. 查询 Goods

```Java
package com.kswl.jdbc.reflection;  
  
import java.sql.Connection;  
import java.sql.DriverManager;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.util.ArrayList;  
import java.util.List;  
  
public class JdbcUtil {  
  
    public static void main(String[] args) {  
  
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";  
        String username = "root";  
        String password = "Fxly2020c&y";  
  
        List<Goods> goods = new ArrayList<Goods>();  
  
        // 加载驱动  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
            Connection conn = DriverManager.getConnection(url, username, password);  
            String sql = "SELECT id,name,number,price,agent_id FROM goods WHERE name LIKE ? AND price > ?";  
            PreparedStatement ps = conn.prepareStatement(sql);  
            ps.setString(1, "%小米%");  
            ps.setDouble(2, 1000.00); // 查询价格在1000元以上的  
            ResultSet rs = ps.executeQuery();  
            while (rs.next()) {  
                Goods g = new Goods();  
                g.setId(rs.getLong("id"));  
                g.setName(rs.getString("name"));  
                g.setNumber(rs.getLong("number"));  
                g.setPrice(rs.getDouble("price"));  
                g.setAgentId(rs.getInt("agent_id"));  
                goods.add(g);  
            }  
            rs.close();  
            ps.close();  
            conn.close();  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
  
        // 输出  
        goods.forEach(System.out::println);  
    }
  
}
```

```
// 输出结果
Goods [id=1, name=小米1, number=46, price=4698.3, agentId=1]
Goods [id=2, name=小米2, number=45, price=1245.84, agentId=1]
Goods [id=3, name=小米3, number=99, price=3406.59, agentId=1]
Goods [id=4, name=小米4, number=10, price=2671.45, agentId=1]
Goods [id=5, name=小米5, number=87, price=3341.94, agentId=1]
Goods [id=8, name=小米8, number=60, price=1836.97, agentId=1]
Goods [id=10, name=小米10, number=39, price=2926.76, agentId=1]
Goods [id=11, name=小米1, number=70, price=4472.33, agentId=2]
Goods [id=12, name=小米2, number=88, price=4493.26, agentId=2]
Goods [id=13, name=小米3, number=93, price=4101.89, agentId=2]
Goods [id=14, name=小米4, number=92, price=1422.37, agentId=2]
Goods [id=15, name=小米5, number=27, price=1943.57, agentId=2]
Goods [id=17, name=小米7, number=84, price=3490.61, agentId=2]
Goods [id=18, name=小米8, number=21, price=2308.11, agentId=2]
Goods [id=19, name=小米9, number=19, price=4036.18, agentId=2]
Goods [id=20, name=小米10, number=45, price=4345.14, agentId=2]
Goods [id=21, name=小米1, number=95, price=2539.82, agentId=3]
Goods [id=22, name=小米2, number=78, price=4048.26, agentId=3]
Goods [id=23, name=小米3, number=44, price=3780.66, agentId=3]
Goods [id=24, name=小米4, number=90, price=4779.84, agentId=3]
Goods [id=25, name=小米5, number=87, price=3599.3, agentId=3]
Goods [id=26, name=小米6, number=52, price=3067.17, agentId=3]
Goods [id=27, name=小米7, number=84, price=4960.45, agentId=3]
Goods [id=28, name=小米8, number=47, price=1528.3, agentId=3]
Goods [id=30, name=小米10, number=75, price=2551.2, agentId=3]
Goods [id=32, name=小米2, number=14, price=4551.07, agentId=4]
Goods [id=33, name=小米3, number=89, price=1662.8, agentId=4]
Goods [id=34, name=小米4, number=94, price=3307.64, agentId=4]
Goods [id=36, name=小米6, number=87, price=3142.26, agentId=4]
Goods [id=37, name=小米7, number=47, price=2463.44, agentId=4]
Goods [id=38, name=小米8, number=23, price=4857.06, agentId=4]
Goods [id=39, name=小米9, number=25, price=4119.23, agentId=4]
Goods [id=40, name=小米10, number=55, price=4704.55, agentId=4]
Goods [id=41, name=小米1, number=33, price=2844.52, agentId=5]
Goods [id=42, name=小米2, number=52, price=4666.95, agentId=5]
Goods [id=43, name=小米3, number=69, price=1792.4, agentId=5]
Goods [id=44, name=小米4, number=83, price=1934.84, agentId=5]
Goods [id=45, name=小米5, number=79, price=4718.57, agentId=5]
Goods [id=46, name=小米6, number=42, price=3322.27, agentId=5]
Goods [id=48, name=小米8, number=86, price=4030.8, agentId=5]
Goods [id=49, name=小米9, number=88, price=3153.75, agentId=5]
Goods [id=50, name=小米10, number=74, price=3472.23, agentId=5]
Goods [id=52, name=小米2, number=67, price=2026.11, agentId=6]
Goods [id=57, name=小米7, number=61, price=4728.28, agentId=6]
Goods [id=58, name=小米8, number=30, price=3964.37, agentId=6]
Goods [id=59, name=小米9, number=17, price=3166.25, agentId=6]
Goods [id=60, name=小米10, number=11, price=2838.07, agentId=6]
Goods [id=62, name=小米2, number=25, price=1762.86, agentId=7]
Goods [id=64, name=小米4, number=38, price=4261.0, agentId=7]
Goods [id=66, name=小米6, number=38, price=4859.88, agentId=7]
Goods [id=67, name=小米7, number=98, price=3950.28, agentId=7]
Goods [id=68, name=小米8, number=74, price=2812.38, agentId=7]
Goods [id=69, name=小米9, number=29, price=3906.93, agentId=7]
Goods [id=70, name=小米10, number=44, price=4717.1, agentId=7]
Goods [id=71, name=小米1, number=93, price=3135.81, agentId=8]
Goods [id=73, name=小米3, number=42, price=2637.98, agentId=8]
Goods [id=74, name=小米4, number=98, price=1769.91, agentId=8]
Goods [id=75, name=小米5, number=33, price=4672.9, agentId=8]
Goods [id=77, name=小米7, number=50, price=3395.12, agentId=8]
Goods [id=78, name=小米8, number=67, price=1818.93, agentId=8]
Goods [id=79, name=小米9, number=39, price=3265.39, agentId=8]
Goods [id=80, name=小米10, number=72, price=4115.45, agentId=8]
Goods [id=81, name=小米1, number=52, price=4788.82, agentId=9]
Goods [id=82, name=小米2, number=56, price=4246.1, agentId=9]
Goods [id=83, name=小米3, number=71, price=2972.1, agentId=9]
Goods [id=84, name=小米4, number=26, price=3840.18, agentId=9]
Goods [id=85, name=小米5, number=16, price=1206.59, agentId=9]
Goods [id=86, name=小米6, number=86, price=2321.33, agentId=9]
Goods [id=88, name=小米8, number=88, price=1484.05, agentId=9]
Goods [id=90, name=小米10, number=60, price=2472.72, agentId=9]
Goods [id=91, name=小米1, number=42, price=1619.23, agentId=10]
Goods [id=92, name=小米2, number=77, price=2755.24, agentId=10]
Goods [id=93, name=小米3, number=29, price=4540.98, agentId=10]
Goods [id=94, name=小米4, number=13, price=1155.57, agentId=10]
Goods [id=95, name=小米5, number=69, price=2090.86, agentId=10]
Goods [id=98, name=小米8, number=73, price=4874.59, agentId=10]
Goods [id=99, name=小米9, number=95, price=3734.31, agentId=10]
Goods [id=702, name=小米2, number=99, price=4290.73, agentId=71]
Goods [id=703, name=小米3, number=14, price=2386.51, agentId=71]
Goods [id=704, name=小米4, number=58, price=4406.69, agentId=71]
Goods [id=705, name=小米5, number=95, price=4127.0, agentId=71]
Goods [id=706, name=小米6, number=41, price=3461.85, agentId=71]
Goods [id=708, name=小米8, number=56, price=2057.03, agentId=71]
Goods [id=710, name=小米10, number=58, price=3007.27, agentId=71]
Goods [id=713, name=小米3, number=94, price=2601.66, agentId=72]
Goods [id=716, name=小米6, number=34, price=3561.79, agentId=72]
Goods [id=717, name=小米7, number=62, price=2956.99, agentId=72]
Goods [id=718, name=小米8, number=91, price=4981.48, agentId=72]
Goods [id=719, name=小米9, number=85, price=4147.57, agentId=72]
Goods [id=720, name=小米10, number=61, price=4297.64, agentId=72]
Goods [id=721, name=小米1, number=84, price=2151.34, agentId=73]
Goods [id=722, name=小米2, number=52, price=2383.08, agentId=73]
Goods [id=723, name=小米3, number=10, price=2123.59, agentId=73]
Goods [id=724, name=小米4, number=27, price=4036.9, agentId=73]
Goods [id=726, name=小米6, number=19, price=2737.76, agentId=73]
Goods [id=728, name=小米8, number=95, price=2051.57, agentId=73]
Goods [id=730, name=小米10, number=53, price=4318.6, agentId=73]
Goods [id=731, name=小米1, number=16, price=2601.85, agentId=74]
Goods [id=732, name=小米2, number=86, price=3085.72, agentId=74]
Goods [id=733, name=小米3, number=38, price=1145.45, agentId=74]
Goods [id=734, name=小米4, number=78, price=1631.29, agentId=74]
Goods [id=735, name=小米5, number=18, price=2188.06, agentId=74]
Goods [id=736, name=小米6, number=70, price=3320.17, agentId=74]
Goods [id=738, name=小米8, number=72, price=1092.6, agentId=74]
Goods [id=739, name=小米9, number=23, price=3383.78, agentId=74]
Goods [id=740, name=小米10, number=19, price=3255.64, agentId=74]
Goods [id=742, name=小米2, number=14, price=1734.76, agentId=75]
Goods [id=744, name=小米4, number=75, price=4834.01, agentId=75]
Goods [id=745, name=小米5, number=14, price=3599.14, agentId=75]
Goods [id=747, name=小米7, number=75, price=2301.98, agentId=75]
Goods [id=748, name=小米8, number=67, price=1801.6, agentId=75]
Goods [id=750, name=小米10, number=16, price=2701.35, agentId=75]
Goods [id=751, name=小米1, number=71, price=3114.55, agentId=76]
Goods [id=752, name=小米2, number=52, price=1659.42, agentId=76]
Goods [id=753, name=小米3, number=60, price=1548.9, agentId=76]
Goods [id=755, name=小米5, number=64, price=3620.76, agentId=76]
Goods [id=756, name=小米6, number=56, price=4542.35, agentId=76]
Goods [id=757, name=小米7, number=92, price=4628.48, agentId=76]
Goods [id=760, name=小米10, number=32, price=4022.42, agentId=76]
Goods [id=761, name=小米1, number=93, price=4076.82, agentId=77]
Goods [id=762, name=小米2, number=29, price=3068.21, agentId=77]
Goods [id=763, name=小米3, number=74, price=4849.05, agentId=77]
Goods [id=764, name=小米4, number=29, price=2141.25, agentId=77]
Goods [id=765, name=小米5, number=59, price=1677.21, agentId=77]
Goods [id=767, name=小米7, number=47, price=3974.12, agentId=77]
Goods [id=770, name=小米10, number=49, price=3392.51, agentId=77]
Goods [id=771, name=小米1, number=48, price=4012.55, agentId=78]
Goods [id=775, name=小米5, number=75, price=4338.89, agentId=78]
Goods [id=776, name=小米6, number=93, price=2879.71, agentId=78]
Goods [id=777, name=小米7, number=81, price=2231.36, agentId=78]
Goods [id=779, name=小米9, number=79, price=1935.92, agentId=78]
Goods [id=780, name=小米10, number=85, price=2353.69, agentId=78]
Goods [id=781, name=小米1, number=63, price=4211.71, agentId=79]
Goods [id=782, name=小米2, number=49, price=4522.41, agentId=79]
Goods [id=783, name=小米3, number=34, price=1281.63, agentId=79]
Goods [id=784, name=小米4, number=93, price=1863.01, agentId=79]
Goods [id=785, name=小米5, number=11, price=4129.73, agentId=79]
Goods [id=786, name=小米6, number=14, price=4457.79, agentId=79]
Goods [id=788, name=小米8, number=50, price=1379.85, agentId=79]
Goods [id=789, name=小米9, number=60, price=1621.15, agentId=79]
Goods [id=790, name=小米10, number=73, price=3210.62, agentId=79]
Goods [id=792, name=小米2, number=48, price=3788.68, agentId=80]
Goods [id=794, name=小米4, number=97, price=4286.41, agentId=80]
Goods [id=795, name=小米5, number=21, price=2776.0, agentId=80]
Goods [id=797, name=小米7, number=63, price=4469.66, agentId=80]
Goods [id=798, name=小米8, number=72, price=4149.19, agentId=80]
Goods [id=800, name=小米10, number=16, price=3864.09, agentId=80]
Goods [id=1401, name=小米1, number=83, price=1359.44, agentId=141]
Goods [id=1404, name=小米4, number=35, price=3432.3, agentId=141]
Goods [id=1405, name=小米5, number=99, price=3843.32, agentId=141]
Goods [id=1406, name=小米6, number=21, price=1648.64, agentId=141]
Goods [id=1407, name=小米7, number=39, price=1725.62, agentId=141]
Goods [id=1408, name=小米8, number=44, price=2850.62, agentId=141]
Goods [id=1411, name=小米1, number=84, price=4860.2, agentId=142]
Goods [id=1412, name=小米2, number=91, price=4500.99, agentId=142]
Goods [id=1413, name=小米3, number=47, price=3830.61, agentId=142]
Goods [id=1414, name=小米4, number=72, price=3691.07, agentId=142]
Goods [id=1415, name=小米5, number=12, price=3835.74, agentId=142]
Goods [id=1416, name=小米6, number=69, price=1257.36, agentId=142]
Goods [id=1417, name=小米7, number=24, price=1392.03, agentId=142]
Goods [id=1418, name=小米8, number=18, price=4129.85, agentId=142]
Goods [id=1419, name=小米9, number=50, price=4752.63, agentId=142]
Goods [id=1421, name=小米1, number=16, price=4584.32, agentId=143]
Goods [id=1422, name=小米2, number=26, price=4649.9, agentId=143]
Goods [id=1423, name=小米3, number=45, price=3238.31, agentId=143]
Goods [id=1424, name=小米4, number=43, price=2326.86, agentId=143]
Goods [id=1425, name=小米5, number=59, price=2654.12, agentId=143]
Goods [id=1426, name=小米6, number=80, price=2370.67, agentId=143]
Goods [id=1430, name=小米10, number=89, price=2807.86, agentId=143]
Goods [id=1431, name=小米1, number=10, price=3004.18, agentId=144]
Goods [id=1432, name=小米2, number=46, price=3410.3, agentId=144]
Goods [id=1433, name=小米3, number=45, price=1535.95, agentId=144]
Goods [id=1434, name=小米4, number=76, price=4946.56, agentId=144]
Goods [id=1435, name=小米5, number=37, price=3131.37, agentId=144]
Goods [id=1436, name=小米6, number=54, price=4488.17, agentId=144]
Goods [id=1437, name=小米7, number=92, price=2443.95, agentId=144]
Goods [id=1438, name=小米8, number=92, price=4667.68, agentId=144]
Goods [id=1439, name=小米9, number=84, price=1750.89, agentId=144]
Goods [id=1440, name=小米10, number=52, price=1687.35, agentId=144]
Goods [id=1442, name=小米2, number=18, price=3097.51, agentId=145]
Goods [id=1443, name=小米3, number=12, price=3258.08, agentId=145]
Goods [id=1445, name=小米5, number=66, price=2932.04, agentId=145]
Goods [id=1446, name=小米6, number=65, price=1025.79, agentId=145]
Goods [id=1448, name=小米8, number=53, price=3296.57, agentId=145]
Goods [id=1449, name=小米9, number=34, price=2942.69, agentId=145]
Goods [id=1452, name=小米2, number=58, price=1495.11, agentId=146]
Goods [id=1453, name=小米3, number=51, price=1763.99, agentId=146]
Goods [id=1454, name=小米4, number=53, price=2741.15, agentId=146]
Goods [id=1455, name=小米5, number=54, price=4204.54, agentId=146]
Goods [id=1456, name=小米6, number=87, price=2829.96, agentId=146]
Goods [id=1457, name=小米7, number=20, price=4211.45, agentId=146]
Goods [id=1458, name=小米8, number=23, price=3046.75, agentId=146]
Goods [id=1459, name=小米9, number=26, price=2150.27, agentId=146]
Goods [id=1460, name=小米10, number=57, price=3577.29, agentId=146]
Goods [id=1461, name=小米1, number=16, price=1869.49, agentId=147]
Goods [id=1463, name=小米3, number=44, price=4346.54, agentId=147]
Goods [id=1464, name=小米4, number=66, price=2898.45, agentId=147]
Goods [id=1466, name=小米6, number=24, price=2601.64, agentId=147]
Goods [id=1467, name=小米7, number=46, price=2710.97, agentId=147]
Goods [id=1468, name=小米8, number=71, price=3696.47, agentId=147]
Goods [id=1469, name=小米9, number=61, price=4543.79, agentId=147]
Goods [id=1470, name=小米10, number=72, price=4877.07, agentId=147]
Goods [id=1471, name=小米1, number=90, price=3656.62, agentId=148]
Goods [id=1472, name=小米2, number=65, price=4725.73, agentId=148]
Goods [id=1473, name=小米3, number=37, price=3581.89, agentId=148]
Goods [id=1474, name=小米4, number=81, price=2903.73, agentId=148]
Goods [id=1476, name=小米6, number=18, price=4081.84, agentId=148]
Goods [id=1478, name=小米8, number=79, price=1619.93, agentId=148]
Goods [id=1479, name=小米9, number=64, price=3505.25, agentId=148]
Goods [id=1480, name=小米10, number=78, price=1977.05, agentId=148]
Goods [id=1481, name=小米1, number=10, price=2795.3, agentId=149]
Goods [id=1482, name=小米2, number=22, price=3235.65, agentId=149]
Goods [id=1483, name=小米3, number=94, price=3145.04, agentId=149]
Goods [id=1484, name=小米4, number=45, price=1141.44, agentId=149]
Goods [id=1485, name=小米5, number=48, price=3469.01, agentId=149]
Goods [id=1486, name=小米6, number=18, price=3580.68, agentId=149]
Goods [id=1487, name=小米7, number=72, price=4167.24, agentId=149]
Goods [id=1488, name=小米8, number=30, price=1280.37, agentId=149]
Goods [id=1490, name=小米10, number=29, price=3768.34, agentId=149]
Goods [id=1491, name=小米1, number=62, price=2607.61, agentId=150]
Goods [id=1492, name=小米2, number=88, price=4766.39, agentId=150]
Goods [id=1493, name=小米3, number=26, price=1760.47, agentId=150]
Goods [id=1495, name=小米5, number=61, price=2709.85, agentId=150]
Goods [id=1496, name=小米6, number=56, price=4146.65, agentId=150]
Goods [id=1497, name=小米7, number=92, price=3623.58, agentId=150]
Goods [id=1498, name=小米8, number=80, price=4221.04, agentId=150]
Goods [id=1499, name=小米9, number=23, price=4365.5, agentId=150]
Goods [id=1500, name=小米10, number=95, price=3106.56, agentId=150]
Goods [id=2101, name=小米1, number=28, price=3556.74, agentId=211]
Goods [id=2102, name=小米2, number=24, price=1632.38, agentId=211]
Goods [id=2103, name=小米3, number=46, price=4779.53, agentId=211]
Goods [id=2104, name=小米4, number=46, price=1936.91, agentId=211]
Goods [id=2105, name=小米5, number=33, price=2403.81, agentId=211]
Goods [id=2106, name=小米6, number=30, price=3593.36, agentId=211]
Goods [id=2107, name=小米7, number=63, price=1633.13, agentId=211]
Goods [id=2108, name=小米8, number=91, price=4547.2, agentId=211]
Goods [id=2109, name=小米9, number=39, price=2409.57, agentId=211]
Goods [id=2110, name=小米10, number=88, price=1735.72, agentId=211]
Goods [id=2111, name=小米1, number=25, price=2695.06, agentId=212]
Goods [id=2112, name=小米2, number=10, price=2474.33, agentId=212]
Goods [id=2113, name=小米3, number=89, price=1591.16, agentId=212]
Goods [id=2117, name=小米7, number=48, price=1908.59, agentId=212]
Goods [id=2118, name=小米8, number=82, price=3189.03, agentId=212]
Goods [id=2119, name=小米9, number=86, price=2346.55, agentId=212]
Goods [id=2120, name=小米10, number=99, price=3497.76, agentId=212]
Goods [id=2121, name=小米1, number=44, price=4698.51, agentId=213]
Goods [id=2122, name=小米2, number=79, price=2151.8, agentId=213]
Goods [id=2123, name=小米3, number=35, price=2972.1, agentId=213]
Goods [id=2124, name=小米4, number=82, price=1270.82, agentId=213]
Goods [id=2126, name=小米6, number=99, price=2715.11, agentId=213]
Goods [id=2127, name=小米7, number=33, price=1638.73, agentId=213]
Goods [id=2128, name=小米8, number=96, price=2961.57, agentId=213]
Goods [id=2129, name=小米9, number=17, price=1231.37, agentId=213]
Goods [id=2130, name=小米10, number=24, price=2896.88, agentId=213]
Goods [id=2131, name=小米1, number=47, price=2345.7, agentId=214]
Goods [id=2132, name=小米2, number=28, price=3308.78, agentId=214]
Goods [id=2133, name=小米3, number=19, price=2414.28, agentId=214]
Goods [id=2138, name=小米8, number=98, price=2259.63, agentId=214]
Goods [id=2140, name=小米10, number=31, price=2078.05, agentId=214]
Goods [id=2141, name=小米1, number=21, price=2794.65, agentId=215]
Goods [id=2142, name=小米2, number=61, price=2117.63, agentId=215]
Goods [id=2143, name=小米3, number=14, price=3761.31, agentId=215]
Goods [id=2144, name=小米4, number=47, price=1109.97, agentId=215]
Goods [id=2146, name=小米6, number=60, price=2461.43, agentId=215]
Goods [id=2147, name=小米7, number=68, price=2303.23, agentId=215]
Goods [id=2148, name=小米8, number=40, price=2245.76, agentId=215]
Goods [id=2149, name=小米9, number=78, price=3971.9, agentId=215]
Goods [id=2150, name=小米10, number=99, price=2673.94, agentId=215]
Goods [id=2151, name=小米1, number=43, price=2828.62, agentId=216]
Goods [id=2152, name=小米2, number=12, price=4587.75, agentId=216]
Goods [id=2153, name=小米3, number=32, price=2654.2, agentId=216]
Goods [id=2154, name=小米4, number=21, price=1825.2, agentId=216]
Goods [id=2155, name=小米5, number=57, price=4567.58, agentId=216]
Goods [id=2156, name=小米6, number=26, price=1363.75, agentId=216]
Goods [id=2157, name=小米7, number=14, price=2492.48, agentId=216]
Goods [id=2158, name=小米8, number=57, price=2098.03, agentId=216]
Goods [id=2159, name=小米9, number=92, price=4036.33, agentId=216]
Goods [id=2161, name=小米1, number=93, price=4979.24, agentId=217]
Goods [id=2162, name=小米2, number=37, price=4976.73, agentId=217]
Goods [id=2163, name=小米3, number=35, price=4032.69, agentId=217]
Goods [id=2164, name=小米4, number=82, price=3739.4, agentId=217]
Goods [id=2167, name=小米7, number=80, price=2395.79, agentId=217]
Goods [id=2168, name=小米8, number=24, price=4338.64, agentId=217]
Goods [id=2169, name=小米9, number=21, price=1288.24, agentId=217]
Goods [id=2171, name=小米1, number=39, price=3844.49, agentId=218]
Goods [id=2173, name=小米3, number=17, price=2451.56, agentId=218]
Goods [id=2174, name=小米4, number=76, price=1552.94, agentId=218]
Goods [id=2175, name=小米5, number=79, price=2041.65, agentId=218]
Goods [id=2177, name=小米7, number=76, price=4945.81, agentId=218]
Goods [id=2179, name=小米9, number=99, price=4271.15, agentId=218]
Goods [id=2182, name=小米2, number=19, price=4107.51, agentId=219]
Goods [id=2183, name=小米3, number=69, price=2759.54, agentId=219]
Goods [id=2184, name=小米4, number=49, price=1019.68, agentId=219]
Goods [id=2185, name=小米5, number=50, price=1536.51, agentId=219]
Goods [id=2187, name=小米7, number=98, price=4726.46, agentId=219]
Goods [id=2188, name=小米8, number=76, price=4153.07, agentId=219]
Goods [id=2189, name=小米9, number=49, price=4219.14, agentId=219]
Goods [id=2190, name=小米10, number=65, price=4136.16, agentId=219]
Goods [id=2191, name=小米1, number=41, price=2705.46, agentId=220]
Goods [id=2193, name=小米3, number=55, price=4360.79, agentId=220]
Goods [id=2194, name=小米4, number=82, price=3656.74, agentId=220]
Goods [id=2195, name=小米5, number=91, price=2541.66, agentId=220]
Goods [id=2198, name=小米8, number=87, price=2185.72, agentId=220]
Goods [id=2199, name=小米9, number=94, price=3742.66, agentId=220]
Goods [id=2200, name=小米10, number=16, price=4772.02, agentId=220]
Goods [id=2802, name=小米2, number=21, price=2625.65, agentId=281]
Goods [id=2803, name=小米3, number=48, price=2551.37, agentId=281]
Goods [id=2804, name=小米4, number=34, price=2157.58, agentId=281]
Goods [id=2805, name=小米5, number=53, price=1265.93, agentId=281]
Goods [id=2806, name=小米6, number=97, price=2306.5, agentId=281]
Goods [id=2808, name=小米8, number=39, price=3828.39, agentId=281]
Goods [id=2809, name=小米9, number=27, price=1572.06, agentId=281]
Goods [id=2810, name=小米10, number=88, price=1024.06, agentId=281]
Goods [id=2811, name=小米1, number=61, price=1364.14, agentId=282]
Goods [id=2812, name=小米2, number=48, price=2428.7, agentId=282]
Goods [id=2813, name=小米3, number=91, price=2828.31, agentId=282]
Goods [id=2814, name=小米4, number=83, price=3345.27, agentId=282]
Goods [id=2816, name=小米6, number=51, price=3674.23, agentId=282]
Goods [id=2818, name=小米8, number=12, price=1922.63, agentId=282]
Goods [id=2819, name=小米9, number=69, price=1691.62, agentId=282]
Goods [id=2820, name=小米10, number=81, price=4583.28, agentId=282]
Goods [id=2822, name=小米2, number=17, price=1333.71, agentId=283]
Goods [id=2824, name=小米4, number=19, price=4744.36, agentId=283]
Goods [id=2825, name=小米5, number=76, price=1016.28, agentId=283]
Goods [id=2827, name=小米7, number=64, price=4038.35, agentId=283]
Goods [id=2828, name=小米8, number=42, price=3581.08, agentId=283]
Goods [id=2829, name=小米9, number=86, price=2096.14, agentId=283]
Goods [id=2830, name=小米10, number=69, price=3649.98, agentId=283]
Goods [id=2832, name=小米2, number=58, price=4810.69, agentId=284]
Goods [id=2833, name=小米3, number=20, price=1942.94, agentId=284]
Goods [id=2834, name=小米4, number=96, price=4751.64, agentId=284]
Goods [id=2836, name=小米6, number=44, price=2539.75, agentId=284]
Goods [id=2837, name=小米7, number=36, price=3342.72, agentId=284]
Goods [id=2838, name=小米8, number=58, price=1957.16, agentId=284]
Goods [id=2840, name=小米10, number=23, price=4343.85, agentId=284]
Goods [id=2841, name=小米1, number=93, price=1895.12, agentId=285]
Goods [id=2842, name=小米2, number=61, price=3124.79, agentId=285]
Goods [id=2845, name=小米5, number=19, price=2193.87, agentId=285]
Goods [id=2847, name=小米7, number=56, price=4964.09, agentId=285]
Goods [id=2848, name=小米8, number=60, price=2657.52, agentId=285]
Goods [id=2849, name=小米9, number=27, price=2629.35, agentId=285]
Goods [id=2850, name=小米10, number=28, price=2526.38, agentId=285]
Goods [id=2851, name=小米1, number=88, price=3226.89, agentId=286]
Goods [id=2852, name=小米2, number=95, price=1451.29, agentId=286]
Goods [id=2853, name=小米3, number=67, price=3604.63, agentId=286]
Goods [id=2854, name=小米4, number=68, price=3866.94, agentId=286]
Goods [id=2856, name=小米6, number=99, price=4397.19, agentId=286]
Goods [id=2857, name=小米7, number=32, price=1929.99, agentId=286]
Goods [id=2858, name=小米8, number=61, price=4431.3, agentId=286]
Goods [id=2860, name=小米10, number=79, price=2959.81, agentId=286]
Goods [id=2861, name=小米1, number=80, price=1040.95, agentId=287]
Goods [id=2862, name=小米2, number=61, price=4191.84, agentId=287]
Goods [id=2863, name=小米3, number=91, price=2758.23, agentId=287]
Goods [id=2864, name=小米4, number=16, price=4830.56, agentId=287]
Goods [id=2865, name=小米5, number=39, price=2470.72, agentId=287]
Goods [id=2867, name=小米7, number=30, price=2359.19, agentId=287]
Goods [id=2868, name=小米8, number=52, price=4298.04, agentId=287]
Goods [id=2869, name=小米9, number=93, price=3700.67, agentId=287]
Goods [id=2870, name=小米10, number=27, price=4140.1, agentId=287]
Goods [id=2871, name=小米1, number=56, price=4969.24, agentId=288]
Goods [id=2872, name=小米2, number=86, price=3646.52, agentId=288]
Goods [id=2873, name=小米3, number=20, price=1110.78, agentId=288]
Goods [id=2874, name=小米4, number=60, price=3416.87, agentId=288]
Goods [id=2875, name=小米5, number=39, price=1709.56, agentId=288]
Goods [id=2876, name=小米6, number=94, price=3590.58, agentId=288]
Goods [id=2877, name=小米7, number=71, price=4257.16, agentId=288]
Goods [id=2878, name=小米8, number=45, price=4373.54, agentId=288]
Goods [id=2879, name=小米9, number=20, price=4507.28, agentId=288]
Goods [id=2880, name=小米10, number=92, price=3219.74, agentId=288]
Goods [id=2881, name=小米1, number=89, price=4114.5, agentId=289]
Goods [id=2882, name=小米2, number=82, price=4405.26, agentId=289]
Goods [id=2883, name=小米3, number=13, price=2900.99, agentId=289]
Goods [id=2885, name=小米5, number=66, price=3239.21, agentId=289]
Goods [id=2888, name=小米8, number=47, price=3548.05, agentId=289]
Goods [id=2890, name=小米10, number=72, price=3189.39, agentId=289]
Goods [id=2891, name=小米1, number=94, price=1075.41, agentId=290]
Goods [id=2893, name=小米3, number=64, price=2410.03, agentId=290]
Goods [id=2894, name=小米4, number=77, price=3672.88, agentId=290]
Goods [id=2895, name=小米5, number=56, price=3080.23, agentId=290]
Goods [id=2896, name=小米6, number=50, price=2319.31, agentId=290]
Goods [id=2897, name=小米7, number=89, price=3230.59, agentId=290]
Goods [id=2899, name=小米9, number=50, price=1037.3, agentId=290]
Goods [id=2900, name=小米10, number=67, price=2269.4, agentId=290]
```

#### 3. 万能查询（基于反射）

通过反射实现，必须要保证类中定义的字段名与查询结果展示的列名称保持一致

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.Method;  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
public class JdbcUtil {  
  
    public static void main(String[] args) {  
  
        String sql = "SELECT id,name,number,price,agent_id FROM goods WHERE name LIKE ? AND price > ?";  
        Object[] params = {  
                "%魅%",  
                4000.00
        };  
        List<Goods> goodsList = query(sql, Goods.class, params);  
        goodsList.forEach(System.out::println); // 输出  
  
    }  
  
    // 万能查询  
    // -Object... params 不定长自变量  
    public static<T> List<T> query(String sql, Class<T> clazz, Object... params) {  
        String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";  
        String username = "root";  
        String password = "Fxly2020c&y";  
  
        List<T> dataList = new ArrayList<>();  
  
        // 加载驱动  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
            Connection conn = DriverManager.getConnection(url, username, password);  
            PreparedStatement ps = conn.prepareStatement(sql);  
            if (params != null && params.length > 0) {  
                for (int i = 0; i < params.length; i++) {  
                    ps.setObject(i + 1, params[i]);  
                }  
            }  
            ResultSet rs = ps.executeQuery();  
            ResultSetMetaData rsmd = rs.getMetaData(); // 得到结果集元数据  
            while (rs.next()) {  
                Constructor<T> c = clazz.getConstructor(); // 获取无参构造  
                T t = c.newInstance(); // 创建对象  
                Field[] fields = clazz.getDeclaredFields(); // 获取类中定义的字段  
                for (Field field : fields) {  
                    String fieldName = field.getName();  
                    String methodName = "set" + fieldName.substring(0, 1).toUpperCase() + fieldName.substring(1);  
                    Method method = clazz.getMethod(methodName, field.getType());  
                    try {  
                        Object value = rs.getObject(fieldName, field.getType());  
                        method.invoke(t, value);  
                    } catch (Exception e) {} // 若报错，则continue  
                }  
                dataList.add(t);  
            }  
            rs.close();  
            ps.close();  
            conn.close();  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
  
        // 输出  
        dataList.forEach(System.out::println);  
  
        return dataList;  
    }
  
}
```

```
// 输出结果
Goods [id=405, name=魅族5, number=49, price=4066.69, agentId=41]
Goods [id=419, name=魅族9, number=24, price=4866.65, agentId=42]
Goods [id=428, name=魅族8, number=28, price=4176.02, agentId=43]
Goods [id=431, name=魅族1, number=33, price=4894.55, agentId=44]
Goods [id=438, name=魅族8, number=89, price=4938.89, agentId=44]
Goods [id=441, name=魅族1, number=54, price=4703.18, agentId=45]
Goods [id=444, name=魅族4, number=46, price=4422.01, agentId=45]
Goods [id=453, name=魅族3, number=91, price=4072.91, agentId=46]
Goods [id=456, name=魅族6, number=58, price=4548.97, agentId=46]
Goods [id=460, name=魅族10, number=29, price=4493.84, agentId=46]
Goods [id=468, name=魅族8, number=91, price=4348.75, agentId=47]
Goods [id=469, name=魅族9, number=13, price=4659.88, agentId=47]
Goods [id=472, name=魅族2, number=74, price=4937.85, agentId=48]
Goods [id=480, name=魅族10, number=21, price=4479.68, agentId=48]
Goods [id=484, name=魅族4, number=26, price=4829.2, agentId=49]
Goods [id=486, name=魅族6, number=16, price=4644.51, agentId=49]
Goods [id=490, name=魅族10, number=32, price=4207.31, agentId=49]
Goods [id=494, name=魅族4, number=74, price=4760.67, agentId=50]
Goods [id=1103, name=魅族3, number=53, price=4633.19, agentId=111]
Goods [id=1106, name=魅族6, number=67, price=4117.46, agentId=111]
Goods [id=1109, name=魅族9, number=70, price=4166.35, agentId=111]
Goods [id=1116, name=魅族6, number=33, price=4887.02, agentId=112]
Goods [id=1123, name=魅族3, number=37, price=4050.62, agentId=113]
Goods [id=1132, name=魅族2, number=33, price=4853.2, agentId=114]
Goods [id=1141, name=魅族1, number=97, price=4056.27, agentId=115]
Goods [id=1151, name=魅族1, number=74, price=4326.8, agentId=116]
Goods [id=1154, name=魅族4, number=82, price=4585.33, agentId=116]
Goods [id=1158, name=魅族8, number=86, price=4624.78, agentId=116]
Goods [id=1167, name=魅族7, number=65, price=4807.59, agentId=117]
Goods [id=1169, name=魅族9, number=59, price=4479.22, agentId=117]
Goods [id=1170, name=魅族10, number=60, price=4223.43, agentId=117]
Goods [id=1175, name=魅族5, number=95, price=4467.01, agentId=118]
Goods [id=1186, name=魅族6, number=29, price=4367.9, agentId=119]
Goods [id=1187, name=魅族7, number=24, price=4506.83, agentId=119]
Goods [id=1190, name=魅族10, number=70, price=4834.56, agentId=119]
Goods [id=1191, name=魅族1, number=51, price=4208.03, agentId=120]
Goods [id=1193, name=魅族3, number=85, price=4201.79, agentId=120]
Goods [id=1195, name=魅族5, number=80, price=4523.19, agentId=120]
Goods [id=1807, name=魅族7, number=30, price=4169.19, agentId=181]
Goods [id=1808, name=魅族8, number=58, price=4519.01, agentId=181]
Goods [id=1824, name=魅族4, number=38, price=4483.54, agentId=183]
Goods [id=1833, name=魅族3, number=28, price=4073.02, agentId=184]
Goods [id=1834, name=魅族4, number=16, price=4705.73, agentId=184]
Goods [id=1837, name=魅族7, number=31, price=4462.61, agentId=184]
Goods [id=1839, name=魅族9, number=96, price=4994.88, agentId=184]
Goods [id=1843, name=魅族3, number=19, price=4764.84, agentId=185]
Goods [id=1844, name=魅族4, number=14, price=4284.15, agentId=185]
Goods [id=1847, name=魅族7, number=66, price=4714.44, agentId=185]
Goods [id=1850, name=魅族10, number=71, price=4130.65, agentId=185]
Goods [id=1860, name=魅族10, number=59, price=4084.58, agentId=186]
Goods [id=1861, name=魅族1, number=22, price=4128.92, agentId=187]
Goods [id=1862, name=魅族2, number=49, price=4769.29, agentId=187]
Goods [id=1867, name=魅族7, number=42, price=4818.83, agentId=187]
Goods [id=1869, name=魅族9, number=64, price=4944.11, agentId=187]
Goods [id=1871, name=魅族1, number=98, price=4747.01, agentId=188]
Goods [id=1875, name=魅族5, number=30, price=4037.3, agentId=188]
Goods [id=1879, name=魅族9, number=68, price=4473.75, agentId=188]
Goods [id=1885, name=魅族5, number=24, price=4260.17, agentId=189]
Goods [id=1886, name=魅族6, number=23, price=4508.82, agentId=189]
Goods [id=2502, name=魅族2, number=24, price=4247.35, agentId=251]
Goods [id=2505, name=魅族5, number=38, price=4682.13, agentId=251]
Goods [id=2508, name=魅族8, number=22, price=4323.17, agentId=251]
Goods [id=2517, name=魅族7, number=77, price=4963.12, agentId=252]
Goods [id=2536, name=魅族6, number=55, price=4909.07, agentId=254]
Goods [id=2543, name=魅族3, number=98, price=4611.59, agentId=255]
Goods [id=2544, name=魅族4, number=42, price=4857.37, agentId=255]
Goods [id=2547, name=魅族7, number=98, price=4980.54, agentId=255]
Goods [id=2557, name=魅族7, number=50, price=4155.81, agentId=256]
Goods [id=2558, name=魅族8, number=43, price=4729.2, agentId=256]
Goods [id=2562, name=魅族2, number=28, price=4152.21, agentId=257]
Goods [id=2565, name=魅族5, number=24, price=4287.21, agentId=257]
Goods [id=2572, name=魅族2, number=35, price=4033.31, agentId=258]
Goods [id=2577, name=魅族7, number=21, price=4884.1, agentId=258]
Goods [id=2587, name=魅族7, number=84, price=4273.06, agentId=259]
Goods [id=2591, name=魅族1, number=66, price=4778.78, agentId=260]
Goods [id=3203, name=魅族3, number=69, price=4412.51, agentId=321]
Goods [id=3210, name=魅族10, number=88, price=4525.62, agentId=321]
Goods [id=3217, name=魅族7, number=22, price=4406.38, agentId=322]
Goods [id=3226, name=魅族6, number=78, price=4896.64, agentId=323]
Goods [id=3228, name=魅族8, number=15, price=4896.68, agentId=323]
Goods [id=3247, name=魅族7, number=50, price=4141.07, agentId=325]
Goods [id=3249, name=魅族9, number=27, price=4706.95, agentId=325]
Goods [id=3250, name=魅族10, number=67, price=4331.38, agentId=325]
Goods [id=3251, name=魅族1, number=69, price=4908.26, agentId=326]
Goods [id=3253, name=魅族3, number=27, price=4681.4, agentId=326]
Goods [id=3257, name=魅族7, number=48, price=4565.0, agentId=326]
Goods [id=3258, name=魅族8, number=72, price=4736.39, agentId=326]
Goods [id=3262, name=魅族2, number=50, price=4958.74, agentId=327]
Goods [id=3264, name=魅族4, number=95, price=4940.26, agentId=327]
Goods [id=3267, name=魅族7, number=82, price=4718.46, agentId=327]
Goods [id=3272, name=魅族2, number=41, price=4974.85, agentId=328]
Goods [id=3281, name=魅族1, number=36, price=4601.86, agentId=329]
Goods [id=3288, name=魅族8, number=64, price=4419.94, agentId=329]
Goods [id=3296, name=魅族6, number=87, price=4073.42, agentId=330]
Goods [id=405, name=魅族5, number=49, price=4066.69, agentId=41]
Goods [id=419, name=魅族9, number=24, price=4866.65, agentId=42]
Goods [id=428, name=魅族8, number=28, price=4176.02, agentId=43]
Goods [id=431, name=魅族1, number=33, price=4894.55, agentId=44]
Goods [id=438, name=魅族8, number=89, price=4938.89, agentId=44]
Goods [id=441, name=魅族1, number=54, price=4703.18, agentId=45]
Goods [id=444, name=魅族4, number=46, price=4422.01, agentId=45]
Goods [id=453, name=魅族3, number=91, price=4072.91, agentId=46]
Goods [id=456, name=魅族6, number=58, price=4548.97, agentId=46]
Goods [id=460, name=魅族10, number=29, price=4493.84, agentId=46]
Goods [id=468, name=魅族8, number=91, price=4348.75, agentId=47]
Goods [id=469, name=魅族9, number=13, price=4659.88, agentId=47]
Goods [id=472, name=魅族2, number=74, price=4937.85, agentId=48]
Goods [id=480, name=魅族10, number=21, price=4479.68, agentId=48]
Goods [id=484, name=魅族4, number=26, price=4829.2, agentId=49]
Goods [id=486, name=魅族6, number=16, price=4644.51, agentId=49]
Goods [id=490, name=魅族10, number=32, price=4207.31, agentId=49]
Goods [id=494, name=魅族4, number=74, price=4760.67, agentId=50]
Goods [id=1103, name=魅族3, number=53, price=4633.19, agentId=111]
Goods [id=1106, name=魅族6, number=67, price=4117.46, agentId=111]
Goods [id=1109, name=魅族9, number=70, price=4166.35, agentId=111]
Goods [id=1116, name=魅族6, number=33, price=4887.02, agentId=112]
Goods [id=1123, name=魅族3, number=37, price=4050.62, agentId=113]
Goods [id=1132, name=魅族2, number=33, price=4853.2, agentId=114]
Goods [id=1141, name=魅族1, number=97, price=4056.27, agentId=115]
Goods [id=1151, name=魅族1, number=74, price=4326.8, agentId=116]
Goods [id=1154, name=魅族4, number=82, price=4585.33, agentId=116]
Goods [id=1158, name=魅族8, number=86, price=4624.78, agentId=116]
Goods [id=1167, name=魅族7, number=65, price=4807.59, agentId=117]
Goods [id=1169, name=魅族9, number=59, price=4479.22, agentId=117]
Goods [id=1170, name=魅族10, number=60, price=4223.43, agentId=117]
Goods [id=1175, name=魅族5, number=95, price=4467.01, agentId=118]
Goods [id=1186, name=魅族6, number=29, price=4367.9, agentId=119]
Goods [id=1187, name=魅族7, number=24, price=4506.83, agentId=119]
Goods [id=1190, name=魅族10, number=70, price=4834.56, agentId=119]
Goods [id=1191, name=魅族1, number=51, price=4208.03, agentId=120]
Goods [id=1193, name=魅族3, number=85, price=4201.79, agentId=120]
Goods [id=1195, name=魅族5, number=80, price=4523.19, agentId=120]
Goods [id=1807, name=魅族7, number=30, price=4169.19, agentId=181]
Goods [id=1808, name=魅族8, number=58, price=4519.01, agentId=181]
Goods [id=1824, name=魅族4, number=38, price=4483.54, agentId=183]
Goods [id=1833, name=魅族3, number=28, price=4073.02, agentId=184]
Goods [id=1834, name=魅族4, number=16, price=4705.73, agentId=184]
Goods [id=1837, name=魅族7, number=31, price=4462.61, agentId=184]
Goods [id=1839, name=魅族9, number=96, price=4994.88, agentId=184]
Goods [id=1843, name=魅族3, number=19, price=4764.84, agentId=185]
Goods [id=1844, name=魅族4, number=14, price=4284.15, agentId=185]
Goods [id=1847, name=魅族7, number=66, price=4714.44, agentId=185]
Goods [id=1850, name=魅族10, number=71, price=4130.65, agentId=185]
Goods [id=1860, name=魅族10, number=59, price=4084.58, agentId=186]
Goods [id=1861, name=魅族1, number=22, price=4128.92, agentId=187]
Goods [id=1862, name=魅族2, number=49, price=4769.29, agentId=187]
Goods [id=1867, name=魅族7, number=42, price=4818.83, agentId=187]
Goods [id=1869, name=魅族9, number=64, price=4944.11, agentId=187]
Goods [id=1871, name=魅族1, number=98, price=4747.01, agentId=188]
Goods [id=1875, name=魅族5, number=30, price=4037.3, agentId=188]
Goods [id=1879, name=魅族9, number=68, price=4473.75, agentId=188]
Goods [id=1885, name=魅族5, number=24, price=4260.17, agentId=189]
Goods [id=1886, name=魅族6, number=23, price=4508.82, agentId=189]
Goods [id=2502, name=魅族2, number=24, price=4247.35, agentId=251]
Goods [id=2505, name=魅族5, number=38, price=4682.13, agentId=251]
Goods [id=2508, name=魅族8, number=22, price=4323.17, agentId=251]
Goods [id=2517, name=魅族7, number=77, price=4963.12, agentId=252]
Goods [id=2536, name=魅族6, number=55, price=4909.07, agentId=254]
Goods [id=2543, name=魅族3, number=98, price=4611.59, agentId=255]
Goods [id=2544, name=魅族4, number=42, price=4857.37, agentId=255]
Goods [id=2547, name=魅族7, number=98, price=4980.54, agentId=255]
Goods [id=2557, name=魅族7, number=50, price=4155.81, agentId=256]
Goods [id=2558, name=魅族8, number=43, price=4729.2, agentId=256]
Goods [id=2562, name=魅族2, number=28, price=4152.21, agentId=257]
Goods [id=2565, name=魅族5, number=24, price=4287.21, agentId=257]
Goods [id=2572, name=魅族2, number=35, price=4033.31, agentId=258]
Goods [id=2577, name=魅族7, number=21, price=4884.1, agentId=258]
Goods [id=2587, name=魅族7, number=84, price=4273.06, agentId=259]
Goods [id=2591, name=魅族1, number=66, price=4778.78, agentId=260]
Goods [id=3203, name=魅族3, number=69, price=4412.51, agentId=321]
Goods [id=3210, name=魅族10, number=88, price=4525.62, agentId=321]
Goods [id=3217, name=魅族7, number=22, price=4406.38, agentId=322]
Goods [id=3226, name=魅族6, number=78, price=4896.64, agentId=323]
Goods [id=3228, name=魅族8, number=15, price=4896.68, agentId=323]
Goods [id=3247, name=魅族7, number=50, price=4141.07, agentId=325]
Goods [id=3249, name=魅族9, number=27, price=4706.95, agentId=325]
Goods [id=3250, name=魅族10, number=67, price=4331.38, agentId=325]
Goods [id=3251, name=魅族1, number=69, price=4908.26, agentId=326]
Goods [id=3253, name=魅族3, number=27, price=4681.4, agentId=326]
Goods [id=3257, name=魅族7, number=48, price=4565.0, agentId=326]
Goods [id=3258, name=魅族8, number=72, price=4736.39, agentId=326]
Goods [id=3262, name=魅族2, number=50, price=4958.74, agentId=327]
Goods [id=3264, name=魅族4, number=95, price=4940.26, agentId=327]
Goods [id=3267, name=魅族7, number=82, price=4718.46, agentId=327]
Goods [id=3272, name=魅族2, number=41, price=4974.85, agentId=328]
Goods [id=3281, name=魅族1, number=36, price=4601.86, agentId=329]
Goods [id=3288, name=魅族8, number=64, price=4419.94, agentId=329]
Goods [id=3296, name=魅族6, number=87, price=4073.42, agentId=330]
```

#### 4. 万能更新（兼万能查询代码优化 - 重复代码分装）

```Java
package com.kswl.jdbc.reflection;  
  
import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.Method;  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
  
public class JdbcUtil {  
  
    private static final String url = "jdbc:mysql://localhost:3306/studyex2?serverTimezone=Asia/Shanghai";  
    private static final String username = "root";  
    private static final String password = "Fxly2020c&y";  
  
    static {  
        try {  
            Class.forName("com.mysql.cj.jdbc.Driver");  
        } catch (ClassNotFoundException e) {  
            System.out.println("驱动程序未加载");  
            throw new RuntimeException(e);  
        }  
    }  
  
    public static void main(String[] args) {  
  
        // 其中，agent_id的构造方法不匹配，故取别名agentId  
        String sql = "SELECT id,name,number,price,agent_id agentId FROM goods WHERE name LIKE ? AND price > ?";  
        Object[] params = {  
                "%魅%",  
                4000.00  
        };  
        List<Goods> goodsList = query(sql, Goods.class, params);  
        goodsList.forEach(System.out::println); // 输出  
  
    }  
  
    // PreparedStatement ps
    private static PreparedStatement createPreparedStatement(Connection conn, String sql, Object... params) {  
        PreparedStatement ps = null;  
        try {  
            ps = conn.prepareStatement(sql);  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        }  
        if (params != null && params.length > 0) {  
            for (int i = 0; i < params.length; i++) {  
                try {  
                    ps.setObject(i + 1, params[i]);  
                } catch (SQLException e) {  
                    throw new RuntimeException(e);  
                }  
            }  
        }  
        return ps;  
    }  
  
    // 关闭连接、执行器、结果集  
    private static void close(AutoCloseable... closeable) {  
        if (closeable != null && closeable.length > 0) {  
            for (AutoCloseable c : closeable) {  
                if (c != null) {  
                    try {  
                        c.close();  
                    } catch (Exception e) {  
                        throw new RuntimeException(e);  
                    }  
                }  
            }  
        }  
    }  
  
    // 万能更新  
    public static int update(String sql, Object... params) {  
  
        int result = 0;  
        Connection conn = null;  
        PreparedStatement ps = null;  
  
        try {  
            conn = DriverManager.getConnection(url, username, password);  
            ps = createPreparedStatement(conn, sql, params);  
            result = ps.executeUpdate();  
        } catch (SQLException e) {  
            throw new RuntimeException(e);  
        } finally {  
            close(ps, conn, null);  
        }  
  
        return result;  
    }  
  
    // 产生一个对象  
    private static<T> T createInstance(Class<T> clazz, ResultSet rs) throws Exception {  
        Constructor<T> c = clazz.getConstructor(); // 获取无参构造  
        T t = c.newInstance(); // 创建对象  
        Field[] fields = clazz.getDeclaredFields(); // 获取类中定义的字段  
        for (Field field : fields) {  
            String fieldName = field.getName();  
            String methodName = "set" + fieldName.substring(0, 1).toUpperCase() + fieldName.substring(1);  
            Method method = clazz.getMethod(methodName, field.getType());  
            try {  
                Object value = rs.getObject(fieldName, field.getType());  
                method.invoke(t, value);  
            } catch (Exception e) {} // 若报错，则continue  
        }  
        return t;  
    }  
  
    // 万能查询  
    // -Object... params 不定长自变量  
    public static<T> List<T> query(String sql, Class<T> clazz, Object... params) {  
  
        List<T> dataList = new ArrayList<>();  
        Connection conn = null;  
        PreparedStatement ps = null;  
        ResultSet rs = null;  
  
        // 加载驱动  
        try {  
            conn = DriverManager.getConnection(url, username, password);  
            ps = createPreparedStatement(conn, sql, params);  
            rs = ps.executeQuery();  
            while (rs.next()) {  
                T t = createInstance(clazz, rs);  
                dataList.add(t);  
            }  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        } finally {  
            close(rs, ps, conn, null);  
        }  
  
        // 输出  
        dataList.forEach(System.out::println);  
  
        return dataList;  
    }
  
}
```