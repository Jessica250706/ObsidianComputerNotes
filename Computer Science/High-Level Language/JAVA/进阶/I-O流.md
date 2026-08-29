# File 类

## 作用

java.io.File 类是对存储在磁盘上的文件信息的一个抽象表示。主要用于文件的创建、查找和删除。

## 使用

### 常用构造方法

```Java
// 通过将给定的字符串路径名转换为抽象路径名来创建File实例
public File(String pathname);

// 通过给定的字符父级串路径和字符串子级路径来创建File实例
public File(String parent, String child);

// 通过父级抽象路径名和字符串子路径创建File实例。
public File(File parent, String child);
```

### 举例

```Java
/* File类构造方法的使用 */

import java.io.File;

public class ex1 {

    public static void main(String[] args) {
        // 方法一
        File file1 = new File("E:\\MyProgram\\test\\testSample1.txt");

        // 方法二
        File file2 = new File("E:\\MyProgram\\test", "testSample1.txt");

        // 方法三
        File parent = new File("E:\\MyProgram\\test");
        File file3 = new File(parent, "testSample1.txt");

    }

}
```

![[image 20.png|image 20.png]]

### 常用方法

```Java
//绝对路径：带有盘符的路径称之为绝对路径
//相对路径：不带盘符的路径称之为相对路径，相对路径相对于当前工程来定位的。

public String getAbsolutePath();  // 获取文件的绝对路径

public String getName(); // 获取文件的名字

public String getPath(); // 获取文件的路径

public File getParentFile(); // 获取文件的父文件

public String getParent(); // 获取文件的父文件路径

public long length(); // 获取文件的大小

public long lastModified(); // 获取文件最后修改时间
```

### 举例

### 文件相关的判断

```Java
public boolean canRead();//是否可读
public boolean canWrite();//是否可写
public boolean exists();//是否存在
public boolean isDirectory();//是否是目录
public boolean isFile();//是否是一个正常的文件
public boolean isHidden();//是否隐藏
public boolean canExecute();//是否可执行
public boolean createNewFile() throws IOException;//创建新的文件
public boolean delete();//删除文件
// 删除文件夹时必须保证文件夹为空，否则将删除失败
public boolean mkdir();//创建目录，一级
public boolean mkdirs();//创建目录，多级
public boolean renameTo(File dest);//文件重命名
```

### 文件列表相关

```Java
// 列出文件夹下所有文件
public File[] listFiles();

// 列出文件夹下所有满足条件的文件
public File[] listFiles(FileFilter filter);
```

## 递归

在方法内部再调用自身就是递归。递归分为直接递归和间接递归。

直接递归就是方法自己调用自己。

间接递归就是多个方法之间相互调用，形成一个闭环，从而构成递归。

使用递归时必须要有出口，也就是使递归停下来。否则，将导致栈内存溢出。

# I/O 流

## 定义

在使用计算机时，你可能遇到过如下场景：

> 当你在编写一个文档时，突然断电了或者计算机蓝屏了，而文档又没有保存。当你重启计算机后，发现文档中修改的那部分内容丢失了，但保存过的内容依然还在。

这是为什么呢？因为我们编写文档的时候，编写的内容是存储在计算机的内存中，这些内容属于临时数据，当我们保存文档后，这些临时数据就写进了磁盘，得以保存。

### 过程分析

编写文档内容存储在内存，换言之，就是向内存写数据

保存文档内容至磁盘，换言之，就是将内存中的数据取出来存储到磁盘

### I/O 的来源

I / O 是 Input 和 Ouput 两个单词的首字母，表示输入输出。其参照物就是内存，写入内存，就是输入，从内存读取数据出来，就是输出。

### JAVA 中的 I/O 流

磁盘和内存是两个不同的设备，它们之间要实现数据的交互，就必须要建立一条通道，在 Java 中实现建立这样的通道的是 I / O 流。Java 中的 I / O 流是按照数据类型来划分的。分别是字节流（缓冲流、二进制数据流和对象流）、字符流。

## 字节流

### 定义

> Programs use byte streams to perform input and output of 8-bit bytes. All  
> byte stream classes are descended from InputStream and OutputStream.  

> 程序使用字节流执行 8 位字节的输入和输出。 所有字节流类均来自 InputStream 和 OutputStream。

### OutputStream 常用方法

```Java
public abstract void write(int b) throws IOException;; //写一个字节

public void write(byte b[]) throws IOException; //将给定的字节数组内容全部写入文件中

//将给定的字节数组中指定的偏移量和长度之间的内容写入文件中
public void write(byte b[], int off, int len) throws IOException;

 public void flush() throws IOException;//强制将通道中数据全部写出
 
public void close() throws IOException;//关闭通道
```

### 文件输出流 FileOutputStream 构造方法

```Java
public FileOutputStream(String name) throws FileNotFoundException; //根据提供的文件路径构建一条文件输出通道
//根据提供的文件路径构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
public FileOutputStream(String name, boolean append) throws 
FileNotFoundException;
 public FileOutputStream(File file) throws FileNotFoundException;//根据提供的文件信息构建一条文件输出通道
//根据提供的文件信息构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
public FileOutputStream(File file, boolean append) throws 
FileNotFoundException;
```

![[image 1 6.png|image 1 6.png]]

### InputStream 常用方法

```Java
public abstract int read() throws IOException; //读取一个字节
public int read(byte b[]) throws IOException; //读取多个字节存储至给定的字节数组中
//读取多个字节按照给定的偏移量及长度存储在给定的字节数组中
public int read(byte b[], int off, int len) throws IOException;
 public void close() throws IOException;//关闭流，也就是关闭磁盘和内存之间的通道
public int available() throws IOException;//获取通道中数据的长度
```

### 文件输入流 FileInputStream 构造方法

```Java
 public FileInputStream(String name) throws FileNotFoundException;//根据提供的文件路径构建一条文件输入通道
public FileInputStream(File file) throws FileNotFoundException;//根据提供的文件信息构建一条文件输入通道
```

![[image 2 6.png|image 2 6.png]]

### 字节流应用场景

Byte streams should only be used for the most primitive I/O.

字节流仅仅适用于读取原始数据（基本数据类型）。

## 字符流

### 定义

1. The Java platform stores character values using Unicode conventions.  
    Character stream I/O automatically translates this internal format to and  
    from the local character set. In Western locales, the local character set is  
    usually an 8-bit superset of ASCII.  
    Java 平台使用 Unicode 约定存储字符值。 字符流 I / O 会自动将此内部格式与本地字符集转换。 在西方语言环境中，本地字符集通常是 ASCII 的 8 位超集。  
    
2. All character stream classes are descended from Reader and Writer. As with  
    byte streams, there are character stream classes that specialize in file I/O:  
    FileReader and FileWriter.  
    所有字符流类均来自 Reader 和 Writer。 与字节流一样，也有专门用于文件 I / O 的字符流类：FileReader 和 FileWriter。  
    

### Writer 常用方法

```Java
public void write(int c) throws IOException; //写一个字符
public void write(char cbuf[]) throws IOException;//将给定的字符数组内容写入到文件中
//将给定的字符数组中给定偏移量和长度的内容写入到文件中
abstract public void write(char cbuf[], int off, int len) throws 
IOException;
public void write(String str) throws IOException;//将字符串写入到文件中
abstract public void flush() throws IOException;//强制将通道中的数据全部写出
abstract public void close() throws IOException;//关闭通道
```

### FileWriter 构造方法

```Java
public FileWriter(String fileName) throws IOException;//根据提供的文件路径构建一条文件输出通道
//根据提供的文件路径构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
public FileWriter(String fileName, boolean append) throws IOException;
public FileWriter(File file) throws IOException;//根据提供的文件信息构建一条文件输出通道
//根据提供的文件信息构建一条文件输出通道，并根据append的值决定是将内容追加到末尾还是直接覆盖
public FileWriter(File file, boolean append) throws IOException;
```

### Reader 常用方法

```Java
public int read() throws IOException; //读取一个字符
public int read(char cbuf[]) throws IOException; //读取字符到给定的字符数组中
//将读取的字符按照给定的偏移量和长度存储在字符数组中
abstract public int read(char cbuf[], int off, int len) throws IOException;
 abstract public void close() throws IOException;//关闭通道
```

### FileReader 构造方法

```Java
public FileReader(String fileName) throws FileNotFoundException;//根据提供的文件路径构建一条文件输入通道
public FileReader(File file) throws FileNotFoundException;//根据提供的文件信息构建一条文件输入通道
```

## 缓冲流

### 定义

1. Most of the examples we've seen so far use unbuffered I/O. This means each  
    read or write request is handled directly by the underlying OS. This can make  
    a program much less efficient, since each such request often triggers disk  
    access, network activity, or some other operation that is relatively  
    expensive.  
    到目前为止，我们看到的大多数示例都使用无缓冲的 I / O。 这意味着每个读取或写入请求均由基础操作系统直接处理。 由于每个这样的请求通常会触发磁盘访问，网络活动或某些其他相对昂贵的操作，因此这可能会使程序的效率大大降低。  
    
2. To reduce this kind of overhead, the Java platform implements buffered I/O  
    streams. Buffered input streams read data from a memory area known as a  
    buffer; the native input API is called only when the buffer is empty. Similarly, buffered output streams write data to a buffer, and the native  
    output API is called only when the buffer is full.  
    为了减少这种开销，Java 平台实现了缓冲的 I / O 流。 缓冲的输入流从称为缓冲区的存储区中读取数据； 仅当缓冲区为空时才调用本机输入 API。 同样，缓冲的输出流将数据写入缓冲区，并且仅在缓冲区已满时才调用本机输出 API。  
    
3. There are four buffered stream classes used to wrap unbuffered streams:  
    BufferedInputStream and BufferedOutputStream create buffered byte streams, while BufferedReader and BufferedWriter create buffered character streams.  
    有四种用于包装非缓冲流的缓冲流类：BufferedInputStream 和 BufferedOutputStream 创建缓冲的字节流，而 BufferedReader 和 BufferedWriter 创建缓冲的字符流。  
    

### BufferedOutputStream 构造方法

```Java
public BufferedOutputStream(OutputStream out);//根据给定的字节输出流创建一个缓冲输出流，缓冲区大小使用默认大小
public BufferedOutputStream(OutputStream out, int size);//根据给定的字节输出流创建一个缓冲输出流，并指定缓冲区大小
```

### BufferedInputStream 构造方法

```Java
public BufferedInputStream(InputStream in); //根据给定的字节输入流创建一个缓冲输入流，缓冲区大小使用默认大小
public BufferedInputStream(InputStream in, int size);//根据给定的字节输入流创建一个缓冲输入流，并指定缓冲区大小
```

### BufferedWriter 构造方法

```Java
public BufferedWriter(Writer out);//根据给定的字符输出流创建一个缓冲字符输出流，缓冲区大小使用默认大小
public BufferedWriter(Writer out, int sz);//根据给定的字符输出流创建一个缓冲字符输出流，并指定缓冲区大小
```

### BufferedReader 构造方法

```Java
public BufferedReader(Reader in); //根据给定的字符输入流创建一个缓冲字符输入流，缓冲区大小使用默认大小
public BufferedReader(Reader in, int sz);//根据给定的字符输入流创建一个缓冲字符输入流，并指定缓冲区大小
```

## 数据流

### 定义

> Data streams support binary I/O of primitive data type values (boolean, char, byte, short, int, long, float, and double) as well as String values. All data  
> streams implement either the DataInput interface or the DataOutput interface. This section focuses on the most widely-used implementations of these interfaces, DataInputStream and DataOutputStream.  

> 数据流支持原始数据类型值（布尔值，char，字节，short，int，long，float 和 double）以及 String 值的二进制 I / O。 所有数据流都实现 DataInput 接口或 DataOutput 接口。 本节重点介绍这些接口的最广泛使用的实现，即 DataInputStream 和 DataOutputStream。

### DataOutput 接口常用方法

```Java
void writeBoolean(boolean v) throws IOException;//将布尔值作为1个字节写入底层输出通道
void writeByte(int v) throws IOException;//将字节写入底层输出通道
void writeShort(int v) throws IOException;//将短整数作为2个字节(高位在前)写入底层输出通道
void writeChar(int v) throws IOException;//将字符作为2个字节写(高位在前)入底层输出通道
void writeInt(int v) throws IOException;//将整数作为4个字节写(高位在前)入底层输出通道
void writeLong(long v) throws IOException;//将长整数作为8个字节写(高位在前)入底层输出通道
void writeFloat(float v) throws IOException;//将单精度浮点数作为4个字节写(高位在前)入底层输出通道
void writeDouble(double v) throws IOException;//将双精度浮点数作为8个字节写(高位在前)入底层输出通道
void writeUTF(String s) throws IOException;//将UTF-8编码格式的字符串以与机器无关的方式写入底层输出通道。
```

### DataOutputStream 构造方法

```Java
public DataOutputStream(OutputStream out);//根据给定的字节输出流创建一个二进制输出流
```

### DataInput 接口常用方法

```Java
boolean readBoolean() throws IOException;//读取一个字节，如果为0，则返回false；否则返回true。
byte readByte() throws IOException;//读取一个字节
int readUnsignedByte() throws IOException;//读取一个字节，返回0~255之间的整数
short readShort() throws IOException;//读取2个字节，然后返一个短整数
int readUnsignedShort() throws IOException;//读取2个字节，返回一个0~65535之间的整数
char readChar() throws IOException;//读取2个字节，然后返回一个字符
int readInt() throws IOException;//读取4个字节，然后返一个整数
long readLong() throws IOException;//读取8个字节，然后返一个长整数
float readFloat() throws IOException;//读取4个字节，然后返一个单精度浮点数
double readDouble() throws IOException;//读取8个字节，然后返一个双精度浮点数
String readUTF() throws IOException;//读取一个使用UTF-8编码格式的字符串
```

### DataInputStream 构造方法

```Java
public DataInputStream(InputStream in);//根据给定的字节输入流创建一个二进制输入流
```

> Notice that DataStreams detects an end-of-file condition by catching  
> EOFException, instead of testing for an invalid return value. All  
> implementations of DataInput methods use EOFException instead of return  
> values.  

> 注意，DataStreams 通过捕获 EOFException 来检测文件结束条件,而不是测试无效的返回值。DataInput 方法的所有实现都使用 EOFException 而不是返回值。

## 对象流

### 定义

> Just as data streams support I/O of primitive data types, object streams  
> support I/O of objects. Most, but not all, standard classes support  
> serialization of their objects. Those that do implement the marker interface  
> Serializable.  

> 正如二进制数据流支持原始数据类型的 I / O 一样，对象流也支持对象的 I / O。大多数（但不是全部）标准类支持其对象的序列化。那些类实现了序列化标记接口 Serializable 才能够序列化。

### ObjectOutput 接口常用方法

```Java
public void writeObject(Object obj) throws IOException;//将对象写入底层输出通道
```

### ObjectOutputSteam 构造方法

```Java
public ObjectOutputStream(OutputStream out) throws IOException;//根据给定的字节输出流创建一个对象输出流
```

p.s. 将一个对象从内存中写入磁盘文件中的过程称之为序列化。序列化必须要求该对象所有类型实现序列化的接口 Serializable

### ObjectInput 接口常用方法

```Java
public Object readObject() throws ClassNotFoundException, IOException;//读取一个对象
```

### ObjectInputSteam 构造方法

```Java
public ObjectInputStream(InputStream in) throws IOException;//根据给定的字节输入流创建一个对象输入流
```

p.s. 将磁盘中存储的对象信息读入内存中的过程称之为反序列化，需要注意的是，反序列化必须保证与序列化时使用的 JDK 版本一致