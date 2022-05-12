# io异常

### jdk1.7之前的异常处理

jdk7之前的处理异常，将流定义在try外面，使流全局可以使用
不使用抛出异常，将异常全部内部处理，
用finally一定会执行的特性，将释放资源的功能写在finally代码块里面，
一共有两个资源需要释放，所以在finally里面写两个try...catch...finally...
第二个要释放的资源要写在第二个finally里面 。保证一定会被执行到

```java
package com.itheima;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * jdk7之前的处理异常，将流定义在try外面，使流全局可以使用
 * 不使用抛出异常，将异常全部内部处理，
 * 用finally一定会执行的特性，将释放资源的功能写在finally代码块里面，
 * 一共有两个资源需要释放，所以在finally里面写两个try...catch...finally...
 * 第二个要释放的资源要写在第二个finally里面 。保证一定会被执行到
 */
public class Demo01 {
    public static void main(String[] args) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream("day019/bbb/hh.jpg");
            fos = new FileOutputStream("day20_IOException/aaa/hhcopy.jpg");


            byte[] bys = new byte[8192];
            int len;
            while ((len = fis.read(bys)) != -1) {
                fos.write(bys, 0, len);
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (fis!=null)
                try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            finally {
                    if (fos!=null)
                        try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```





### jdk1.7之后的异常处理

jdk7之后的异常处理，将异常都try catch住，不抛出，同时try.. with...resource可以自动释放资源，不用再手动释放
将定义流的代码放在try(   )中的括号里面，其他的都和try...catch...finally...一样。

```java
package com.itheima;


import java.io.*;

public class Demo0 {
    public static void main(String[] args) {
        //将定义写在try的括号中
        //定义字节输入流 与目标文件相关联
        //定义字节输出流，与目标文件相关联
        try (FileInputStream fis = new FileInputStream("day019/bbb/hh.jpg");

             FileOutputStream fos = new FileOutputStream("day20_IOException/aaa/hhcopy.jpg");) {


            //定义字节数组 ，用来存储读到的字节数据
            byte[] bys = new byte[8192];
            //定义int类型的变量，用来存储读取到的字节个数
            int len;
            //循环读字节数据
            while ((len = fis.read(bys)) != -1) {
                //写字节数据
                fos.write(bys, 0, len);
            }
            //抓住异常，处理异常
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

# Properties类

Properties继承了HashTable类

他有基本的map操作

额外的功能是可以对文件进行__键值对的写入和读取__

Properties也是一个属性集,可以用来加载文件中的数据(键值对的形式存储), __并且Properties属性集中每个键及其对应值都是一个字符串。__

### 构造方法

```java
Properties pro = new Properties();
```



```java
public Object setProperty(String key, String value) 保存一对属性。  
public String getProperty(String key)  使用此属性列表中指定的键搜索属性值。
public Set<String> stringPropertyNames() 所有键的名称的集合。
```

```java
public class Test3_操作文件 {
    public static void main(String[] args) throws Exception{
        /*
            public void store(OutputStream out,String comments)：用字节输出流写出键值对
            public void store(Writer writer,String comments):用字符输出流写出键值对。
         */
        // 1.创建一个属性集(Properties对象)
        Properties pro = new Properties();

        // 2.存储键值对
        pro.setProperty("k1", "v1");
        pro.setProperty("k2", "v2");
        pro.setProperty("k3", "v3");
        pro.setProperty("k4", "v4");
        pro.setProperty("k5", "v5");

        // 3.调用store方法保存键值对到件中
        //pro.store(new FileOutputStream("day11\\aaa\\b.properties"),"itheima");
        pro.store(new FileWriter("day11\\aaa\\b.properties"),"itcast");
    }
}
```



# 高级流

## 缓冲流

缓冲流,也叫高效流，是对4个基本的`FileXxx` 流的增强，所以也是4个流，按照数据类型分类：

* **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
* **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，__通过缓冲区读写，减少系统IO次数__，从而提高读写的效率。

<img src="/Users/xuhui/Desktop/就业班资料/就业班作业/day11-属性集-缓冲流-转换流-序列化流/01_笔记/img/image-20210331101909243.png" alt="image-20210331101909243" style="zoom: 200%;" />





能够使用字节缓冲流读取数据到程序
    BufferedInputStream:
        public BufferedInputStream(InputStream is);
        int read();读一个字节
        int read(byte[] bys);读一个字节数组

能够使用字节缓冲流写出数据到文件
    BufferedOutputStream:
        public BufferedOutputStream(OutputStream os);
        void write(int b);写一个字节
        void write(byte[] bys,int off,int len);写指定范围的字节数组

```java
public class Test1_普通字节流一次读写一个字节拷贝文件 {
    public static void main(String[] args) throws Exception {
        // 0.获取当前系统时间距离标准基准时间的毫秒值
        long start = System.currentTimeMillis();

        // 1.创建字节输入流对象,关联数据源文件路径
      FileInputStream fis = new FileInputStream("day11\\bbb\\jdk9.exe");

        // 2.创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day11\\bbb\\jdk9Copy1.exe");

        // 3.定义一个int类型变量,用来存储读取到的字节数据
        int b;

        // 4.循环读取字节数据
        while ((b = fis.read()) != -1) {
            // 5.写出字节数据
            fos.write(b);
        }

        // 6.关闭流,释放资源
        fos.close();
        fis.close();

        // 7.获取当前系统时间距离标准基准时间的毫秒值
        long end = System.currentTimeMillis();
        System.out.println("总共花了:" + (end - start) + "毫秒");// 大概十几分钟

    }
```



能够明确字符缓冲流的作用和基本用法
    BufferedReader:
        public BufferedReader(Reader r);
        int read();读一个字符
        int read(char[] chs);读一个字符数组

     BufferedWriter:
        public BufferedWriter(Write w);
        void write(int b);写一个字符
        void write(char[] bys,int off,int len);写指定范围的字符数组

能够使用缓冲流的特殊功能
    BufferedReader: public String readLine();读一行字符串数据
    BufferedWriter: public void newLine();根据系统写行分隔 

```java
public class Test1_BufferedReader类 {
    public static void main(String[] args) throws Exception{
        // public String readLine(); 读取一行数据,读取到文件的末尾返回null
        // 1.创建BufferedReader对象,关联数据源文件路径
        FileReader fr = new FileReader("day11\\ccc\\a.txt");
        BufferedReader br = new BufferedReader(fr);

        // 2.读一行字符串数据
        //System.out.println(br.readLine());
        //System.out.println(br.readLine());
        //System.out.println(br.readLine());
        //System.out.println(br.readLine());
        //System.out.println(br.readLine());// null
        // 定义一个String类型的变量,用来存储读取到的行数据
        String line;

        // 循环读
        while ( (line = br.readLine()) != null){
            System.out.println(line);
        }

        // 3.关闭流,释放资源
        br.close();

    }
```



# 编码表

各方的软件用不同的编码制作文本，也用不同的读码方式读取文本

只有__按照相同的规则编码和解码才不会出现乱码__，不然就会有错误

举例

使用utf-8编码，并且使用utf-8读码时才会正确读取到内容，

常见的编码方式有

+ utf-8

+ Gbk

+ Unicode
+ ISO-8859-1



# 装饰者设计模式

能够理解装饰模式的实现步骤
    使用场景: 在不改变原类,不使用继承的环境下,对某个对象的方法进行增强
    使用步骤:
        1.装饰类和被装饰类必须实现相同的接口
        2.在装饰类中必须传入被装饰类的引用
        3.在装饰类中对需要增强的方法进行增强
        4.在装饰类中对不需要增强的方法就调用被装饰类中同名的方法

用了多态的方式，不需要增强的方法只需要将原类作为参数传进去就可以使用到被装饰类同名的方法了

需要增强的装饰类可以进行复核自己要求的增强，

```java
public interface FindHappy {
    void happy();
}
```

```java
public class HappyPlus implements FindHappy{
    // 在装饰类中必须传入被装饰类的引用(对象)
    FindHappy findHappy;

    public HappyPlus(FindHappy findHappy) {
        this.findHappy = findHappy;
    }

    @Override
    public void happy() {
        // 在装饰类中对需要增强的方法进行增强
        System.out.println("约好人,开好房间...");
        // 被增强的对象调用happy方法
        findHappy.happy();
        System.out.println("打扫房间...");
    }
}
```

```java
public class JinLian implements FindHappy{
    @Override
    public void happy() {
        System.out.println("金莲在happy...");
    }
}

```

```java
public class YanPoXi implements FindHappy{
    @Override
    public void happy() {
        System.out.println("阎婆惜在happy...");
    }
}

```

```java
public class Test {
    public static void main(String[] args) {
        // 创建金莲对象,调用方法
        JinLian jinLian = new JinLian();
        //jinLian.happy();

        // 创建阎婆惜对象,调用方法
        YanPoXi yanPoXi = new YanPoXi();
        //yanPoXi.happy();

        // 创建装饰类
        HappyPlus happyPlus = new HappyPlus(jinLian);
        happyPlus.happy();

        happyPlus.findHappy = yanPoXi;
        happyPlus.happy();
    }
```



# 第三方的io包

#### commons-io工具包的概述

commons-io是apache开源基金组织提供的一组有关IO操作的类库，可以挺提高IO功能开发的效率。commons-io工具包提供了很多有关io操作的类，见下表：

| 包                                  | 功能描述                                     |
| ----------------------------------- | :------------------------------------------- |
| org.apache.commons.io               | 有关Streams、Readers、Writers、Files的工具类 |
| org.apache.commons.io.input         | 输入流相关的实现类，包含Reader和InputStream  |
| org.apache.commons.io.output        | 输出流相关的实现类，包含Writer和OutputStream |
| org.apache.commons.io.serialization | 序列化相关的类                               |

#### commons-io工具包的使用

步骤：

1. 下载commons-io相关jar包；http://commons.apache.org/proper/commons-io/
2. 把commons-io-2.7.jar包复制到指定的Module的lib目录中
3. 将commons-io-2.7.jar加入到classpath路径中----选中jar包 , 右键__add as library__, 确认

#### commons-io工具包中的常用类

- commons-io提供了一个工具类  org.apache.commons.io.IOUtils，封装了大量IO读写操作的代码。其中有两个常用方法：

> 1. public static int copy(InputStream in, OutputStream out)； 把input输入流中的内容拷贝到output输出流中，返回拷贝的字节个数(适合文件大小为2GB以下)
> 2. public static long copyLarge(InputStream in, OutputStream out)；把input输入流中的内容拷贝到output输出流中，返回拷贝的字节个数(适合文件大小为2GB以上)

文件复制案例演示：

~~~java
/**
 * @Author xuhui
 * @Date 2021/7/8 16:19
 */
public class Test1_IOUtils {
    public static void main(String[] args) throws Exception{
        // 创建字节输入流对象,关联数据源文件路径
        FileInputStream fis = new FileInputStream("day11\\aaa\\hb.jpg");
        // 创建字节输出流对象,关联目的地文件路径
        FileOutputStream fos = new FileOutputStream("day11\\fff\\hbCopy1.jpg");
        // 使用IOUtils调用copy方法进行拷贝
        IOUtils.copy(fis,fos);
        // 释放资源
        fos.close();
        fis.close();

    }
}

~~~

#### 工具类org.apache.commons.io.FileUtils

- public static void copyFileToDirectory(final File srcFile, final File destFile) //复制文件到另外一个目录下。
- public static void copyDirectoryToDirectory( File file1 ,File file2 );//复制file1目录到file2位置。

案例演示：

~~~java
import org.apache.commons.io.FileUtils;

import java.io.File;

/**
 * @Author xuhui
 * @Date 2021/7/8 16:23
 */
public class Test2_FileUtils {
    public static void main(String[] args) throws Exception{
        File f1 = new File("day11\\aaa\\hb.jpg");
        File f2 = new File("day11\\fff");
        FileUtils.copyFileToDirectory(f1,f2);
    }
}
~~~



