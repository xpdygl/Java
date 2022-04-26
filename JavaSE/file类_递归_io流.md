## file类

### 构造方法

```java
				File f1 = new File("G:\\szitheima117\\day10\\aaa\\a.txt");// 表示文件路径
        File f2 = new File("G:\\szitheima117\\day10\\aaa");// 表示文件夹路径
        System.out.println("f1:" + f1);
        System.out.println("f2:" + f2);
```



### 路径

绝对路径：从盘符开始的路径，这是一个完整的路径。

相对路径：**相对于项目目录的路径**，这是一个便捷的路径，**开发中经常使用**。

不同的电脑上的绝对路径一般都是不同的，但是只要在同一个项目里相对路径就是相同的，所以一般都是用相对路径。

### 方法

- `public String getAbsolutePath() ` ：返回此File的绝对路径名字符串。
- ` public String getPath() ` ：将此File转换为路径名字符串。 **构造路径**
- `public String getName()`  ：返回由此File表示的文件或目录的名称。  
- `public long length()`  ：返回由此File表示的文件的长度。 **不能获取目录的长度。**





#### 判断功能的方法

- `public boolean exists()` ：此File表示的文件或目录是否实际存在。

- `public boolean isDirectory()` ：此File表示的是否为目录。

- `public boolean isFile()` ：此File表示的是否为文件。

- 注意: 如果File对象表示的路径不存在,以上三个方法的返回值都是false



#### 创建删除功能的方法

- `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。 
- `public boolean delete()` ： **删除文件,或者删除空文件夹, 不能删除非空文件夹**
- `public boolean mkdir()` ：创建由此File表示的目录。
- `public boolean mkdirs()` ：创建由此File表示的目录，包括任何必需但不存在的父目录。





####  遍历目录方法

- `public String[] list()` ：获取File目录中的所有子文件或子目录的名称。


- `public File[] listFiles()` ：获取File目录中的所有子文件或子目录的路径。



## 递归

- 生活中的递归:  放羊-->赚钱-->盖房子-->娶媳妇-->生娃-->放羊-->赚钱-->盖房子-->娶媳妇-->生娃-->放羊...
- 程序中的递归:  **方法自己调用自己**
- **注意:**
  - 1.递归没有出口,就会报栈内存溢出错误StackOverflowError
  - 2.出口不能太晚了,否则也会报栈内存溢出错误StackOverflowError



一直调用自己并将出口放的太晚就会一直在堆内存里开辟新的内存空间，所以也会报栈内存溢出错误。



### 2 递归求阶乘

#### 需求

- 计算n的阶乘

- **阶乘**：所有小于及等于该数的正整数的积。

```java
n的阶乘：n! = n * (n-1) *...* 3 * 2 * 1 
```

n的阶乘 = n * (n - 1)的阶乘，所以可以把阶乘的操作定义成一个方法，递归调用。

```
推理得出：n! = n * (n-1)!
```

### 实现

```java
/**
 * @Author PengZhiLin
 * @Date 2021/7/4 10:14
 */
public class Test2_计算n的阶乘 {
    public static void main(String[] args) {
        System.out.println(jieCheng(5));// 120
    }

    /**
     * 计算n的阶乘
     *
     * @param n
     * @return 阶乘
     */
    public static int jieCheng(int n) {
        // 出口
        if (n == 1){
            return 1;
        }

        // 规律
        return n * jieCheng(n - 1);
    }
}

```





## IO流

I  ：input  从写东西进内存 write

O：output 从内存里读出数据 read





#### IO的分类

- **按类型分:**
  - 字节流: 以字节为基本单位进行读写数据
    - 字节输入流: 以字节为基本单位进行读数据
    - 字节输出流: 以字节为基本单位进行写数据
  - 字符流: 以字符为基本单位进行读写数据
    - 字符输入流: 以字符为基本单位进行读数据
    - 字符输出流: 以字符为基本单位进行写数据
- 按流向分:
  - 输入流: 读数据
    - 字节输入流: 以字节为基本单位进行读数据
    - 字符输入流: 以字符为基本单位进行读数据
  - 输出流: 写数据
    - 字节输出流: 以字节为基本单位进行写数据
    - 字符输出流: 以字符为基本单位进行写数据

#### IO的顶层父类

- 字节输入流: 顶层父类是InputStream抽象类,常见子类FileInputStream
- 字节输出流: 顶层父类是OutputStream抽象类,常见子类FileOutputStream
- 字符输入流: 顶层父类是Reader抽象类,常见子类FileReader
- 字符输出流: 顶层父类是Writer抽象类,常见子类FileWriter



## 字节流

### 字节输出流



使用完必须刷新或者释放，不然操作不会成功。

在服务器中一般不释放，因为服务器的程序要一直运行。

#### OutputStream类的常用方法

- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。    
- `public abstract void write(int b)` ：写出一个字节数据到目的地文件中。
- `public void write(byte[] b)`：将 b.length字节写出到目的地文件中。  
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。 

#### FIleOutputStream



写指定长度的字节数组

public class Test3_写指定长度的字节数组 {
    public static void main(String[] args) throws Exception{
        // 写指定长度字节数组: public void write(byte[] b, int off, int len) ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。
        // 1.创建一个字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day10\\bbb\\f.txt");

```java
    // 2.写字节数组----->abcd
    byte[] bys = {97,98,99,100,49,50,51};
    fos.write(bys,0,4);

    // 3.关闭流,释放资源
    fos.close();

```
```java
public class Test4_追加续写 {
    public static void main(String[] args)throws Exception {
        // 1.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day10\\bbb\\g.txt",true);
        // 2.写字节数组----->abcd
        byte[] bys = {97,98,99,100,49,50,51};
        fos.write(bys,0,4);
        // 3.关闭流,释放资源
        fos.close();
    }
}
文件中原有的数据是: 123
执行该程序之后文件中的数据是:123abcd
```

#### 字节输入流

### FileInputStream类的常用方法

- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。    
- `public abstract int read()`： 读一个字节,返回这个字节的数据。 
- `public int read(byte[] b)`： 读一个字节数组长度个字节,把读取到的字节数据存储到数组中,返回读取到的字节个数



```java
  FileInputStream fis = new FileInputStream("day10\\ccc\\a.txt");
// 定义一个int类型的变量,用来存储读取到的字节数据
        int b;

        // 由于读取的次数不确定,所以使用while循环
        // fis读一个字节,把读取到的这个字节赋值给b,再拿b与-1进行比较
        while ( (b = fis.read()) != -1){
            System.out.println("b:"+b);
        }


        // 3.关闭流,释放资源
        fis.close();

```



## 字符流

由于各个文件的编码问题，字节流在读中英文混合的文章时会出现乱码

用字符流来读取或写入就不会出现这种问题

###  字符输入流【Reader】

#### 字符输入流Reader类的概述

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法。

#### 字符输入流Reader类的常用方法

- `public void close()` ：关闭此流并释放与此流相关联的任何系统资源。    
- `public int read()`： 从输入流读取一个字符,读取文件的末尾返回-1。 
- `public int read(char[] cbuf)`： 从输入流中读取一些字符，并将它们存储到字符数组 cbuf中,返回读取到的字符个数,读取到文件的末尾返回-1 。



```java
  public static void main(String[] args) throws Exception {
        // 读一个字符数组:public int read(char[] cbuf)： 
    		//从输入流中读取一些字符，并将它们存储到字符数组 cbuf中,返回读取到的字符个数,读取到文件的末尾返回-1 。
        // 1.创建字符输入流对象,关联数据源文件路径
        FileReader fr = new FileReader("day10\\ddd\\a.txt");

				// 定义一个char类型的数组,用来存储读取到的字符数据
        char[] chs = new char[2];

        // 定义一个int变量,用来存储读取到的字符个数
        int len;
        // 循环
        while ((len = fr.read(chs)) != -1) {
            System.out.println(new String(chs,0,len));
        }

        // 3.关闭流,释放资源
        fr.close();


```

###  FileWriter类

- 概述: `java.io.FileWriter类`继承Writer类,所以也是一个字符输出流,可以用来写字符数据到目的地文件中

- 构造方法:

  - `public FileWriter(File file); 创建字符输出流对象,通过File参数指定目的地文件路径;`

  - `public FileWriter(String path); 创建字符输出流对象,通过String参数指定目的地文件路径;`

    - **注意:**
      - 如果指定的文件存在,就会清空该文件中的数据
      - 如果指定的文件不存在,就会创建一个新的空文件

  - `public FileWriter(File file,boolean append); 创建字符输出流对象,通过File参数指定目的地文件路径;`

  - `public FileWriter(String path,boolean append); 创建字符输出流对象,通过String参数指定目的地文件路径;`

    - **注意:**
      - 如果指定的文件存在,并且第二个参数为true,就不会清空源文件中的数据,如果第二个参数为false,就会清空
      - 如果指定的文件不存在,就会创建一个新的空文件

#### 写指定的字符串

```java
public class Test5_写指定范围的字符串 {
    public static void main(String[] args) throws Exception{
        // 写指定长度字符串: public void write(String str,int off,int len) ：写出一个字符串的一部分。
        // 1.创建字符输出流对象,关联目的地文件路径
        FileWriter fw = new FileWriter("day10\\eee\\g.txt");

        // 2.写指定范围的字符串---->黑马程序员
        String str = "itheima黑马程序员itcast";
        fw.write(str,7,5);


        // 3.关闭流,释放资源
        fw.close();

    }
}
文件中的数据:黑马程序员
```



