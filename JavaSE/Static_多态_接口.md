## static概述

有些时候，我们希望在类中定义一些能够被类的所有对象所共享的全局内容，这些内容仅在内存中存储一份，以便于我们对这些共享内容的管理和维护，同时也能够节省更多的内存空间。

静态，就是 Java 提供给我们解决上述问题的一种机制，使用 static 关键字修饰的成员（包括成员变量、成员方法、代码块和内部类）就是类的静态成员。静态成员最大的特点就是它属于类，而不是对象。



__我们可以在不创建对象的情况下，直接通过类访问静态成员。__




需要注意的是，静态成员是随着类的加载而存在的，这意味着静态成员始终优先于对象存在。

​	静态成员的好处

		 全局的，可以被所有对象所共享，在内存中只存储一份，更节省内存空间。
   + ​    可以直接通过类访问而不用创建对象，编码效率更高。

静态成员的缺点



+ ​	静态成员是属于类的，类不被卸载，静态成员就一直存在。而实例成员会随着对象的销毁一并被 GC 回收。

  

静态代码块：使用 static 修饰的构造代码块就是静态代码块，静态代码块是类被加载时执行，且仅执行一次。

## 静态方法



静态方法的定义（静态方法中不能使用 this 关键字，因为 this 表示当前对象，而__静态方法存在时还没有对象！__

```java
class ChinesePeople {
  //说话
  public static void speak() {        
    System.out.println("说普通话");
  }
}
```



## 静态方法的访问

```java
public static void main(String[] args) {
  //通过类名直接调用静态方法   
  ChinesePeople.speak(); //说普通话
}

```





##  静态方法的注意事项

+ 在静态方法中不能使用this关键字。
+ 在静态方法中只能访问静态成员。所有非静态的成员都不可以访问
+ 调用静态方法可以直接用类名调用







## 接口



​		接口，是 Java 语言中的一种数据类型，如果说类中封装了成员变量、构造方法和成员方法，那么接口主要就是用于封装抽象方法的。和类一样，接口也会被编译成 class 字节码文件。

​		接口不能创建对象。接口存在的意义就是给子类实现（类似于继承），一个类实现了接口，就必须要实现接口中所有的抽象方法，除非这个类是一个抽象类。

```java
class  interface A{
  定义常量
  抽象方法
  默认方法
  静态方法
}
```

接口中的常量必须是公开的静态常量

### 定义抽象方法

```java
public interface MyInterface {
  //抽象方法（完整格式）    
  public abstract void method1();
  
  //抽象方法（简化格式）   
  void method2();
}
```

### 定义默认方法

```java
public interface MyInterface {
  //默认方法（完整格式）    
  public default void method3() {
    System.out.println("接口中的默认方法 method3");
  }    
  //默认方法（简化格式）    
  default void method4() {        
    System.out.println("接口中的默认方法 method4");   
  }
}

```

### 定义静态方法

```java
public interface MyInterface {
  //静态方法（完整格式）    
  public static void method5() {       
    System.out.println("接口中的默认方法 method5");
  }    
  //静态方法（简化格式）    
  static void method6() {        
    System.out.println("接口中的默认方法 method6");
  }
}

```



## 接口的实现

类和类之间是继承关系，接口和类之间是实现关系。

```java
public class 类名 implements 接口名 {
  
}
```

    - 实现类是普通类：必须实现接口中所有的抽象方法，默认方法可以选择性重写，不重写的话就是直接继承下来。
    - 实现类是抽象类：可以不重写接口中的抽象方法，因为抽象类中允许有抽象方法的存在。
    - 接口中的静态方法只属于接口，不能被继承。直接用接口名访问静态方法即可



父类的成员方法和接口的默认方法重名时，优先执行父类的成员方法

### 接口的多实现

java中一个类只能有一个父类，但是可以有多个接口

语法格式

```java
class 子类 extends 父类 implements 接口A, 接口B, 接口C {

}
```



### 接口和抽象类的区别

|          | 抽象类                           | 接口           |
| -------- | -------------------------------- | -------------- |
| 成员方法 | 可以有抽象方法，也可以有普通方法 | 只能有抽象方法 |
| 成员变量 | 可以是变量，也可以是常量         | 只能是常量     |
| 构造方法 | 有构造方法                       | 没有构造方法   |

从理念上来说，抽象类是对根源的抽象，接口是动作的抽象

抽象类定义是一个体系中的共性内容

接口是功能的集合，是一个体系中额外的功能，是暴露出来的功能

抽象类表示的是，这个对象是什么。接口表示的是

eg：男人和女人 他们抽象出来共有的特性是人，他们可以干一切人的功能

人可以吃东西，狗也可以吃东西，这时我们就可以抽象出一个接口，让他们都可以吃东西。





## 多态

### 多态的前提条件



   - 1、要有继承或实现关系

   - 2、要有方法的重写

   - 3、父类引用指向子类对象
          
          多态的语法格式

     ​      父类类型 对象名 = new 子类类型();

     Ps:要用大驼峰命名法     

```java
Fu f = new Zi();
```

### 多态的弊端

多态的弊端就是无法访问到子类的特有成员

### 多态的语法强制转换

通过强制转换就可以调用子类的特有方法。

```java
 public static void main(String[] args) {
        Animal a = new Cat();
        a.eat();
        Animal b =new Dog();
        b.eat();
        //判断某个对象是否属于某一种类型
        if (a instanceof Cat) {

            Cat c = (Cat)a;//强制装换
            c.catchMouse();
        }
        if (b instanceof Dog){
            Dog d = (Dog)b;
            d.lookHome();
        }
    }

```







## 多态的应用场景

- 变量多态

     就是平常使用的多态

  ```java
  Animal A = new cat();
  //这就是变量多态
  ```

- 参数多态

  使用invokeEat方法时需要传参，传递参数时可以使用多态。

  ```java
   invokeEat(new Cat());
   invokeEat(new Dog());
   
   public static void invokeEat(Animal a) {
           a.eat();
       }
  ```

  

- 返回值多态



调用createAnimal时会产生返回值，直接将传递new Dog( )给主函数，

然后用Animal  a2来接收

```java
     //返回值多态
        Animal a2 = createAnimal();
    }

    public static Animal createAnimal() {
        //return new Cat();
        return new Dog();
    }
```



## 练习

```chinese
/**
 * 语法点：接口，多态
 * 需求：1.定义一个父类Animal 包含name,weight属性
 * 和一个抽象的eat方法,
 * 2.定义两个子类Dog和Cat,Dog特有方法lookHome,
 * Cat特有方法catchMouse;并且重写eat方法,Dog吃骨头,Cat吃鱼
 * 3.要求:使用多态形式创建Dog和Cat对象,
 * 调用eat方法,并且使用向下转型,
 * 如果是Cat类型调用catchMouse功能,如果是Dog类型调用lookHome功能
 */
```

__多态时，除了成员方法访问的是子类的，其他都是父类的。__

```java

public abstract class Animal {
    String name;
    String weight;

    public abstract void eat();
}

public class Cat extends Animal {
    public Cat() {
    }

    @Override
    public void eat() {
        System.out.println("🐈吃🐟");
    }

    public void catchMouse(){
        System.out.println("狂抓老鼠🐭");
    }
}

public class Dog extends Animal{
    @Override
    public void eat() {
        System.out.println("🐶吃骨头");
    }
    public void lookHome(){
        System.out.println("好狗，好好看家");
    }
}

public class demo04 {
    public static void main(String[] args) {
        Animal a = new Cat();
        a.eat();
        Animal b =new Dog();
        b.eat();
        //判断某个对象是否属于某一种类型
        if (a instanceof Cat) {
            Cat c = (Cat)a;//强制转换
            c.catchMouse();
        }
        if (b instanceof Dog){
            Dog d = (Dog)b;
            d.lookHome();
        }
    }
}

```

## 内部类

创建内部类的格式

外部类.内部类 对象名 = new 外部类().new 内部类();

```java
public class inner {
    public static void main(String[] args) {
        // 创建内部类的格式
        // 外部类.内部类 对象名 = new 外部类().new 内部类();
        Outer.Inner oi = new Outer().new Inner();
        oi.show();
    }
}

class Outer {
    public void method() {

    }
    class Inner {
        public void show() {
            System.out.println("这是内部类");
        }
    }
}
```





## 匿名内部类



 * 匿名内部类对象，体现的是一种临时的思想

 * 匿名：因为这个临时的子类（实现类）没有名字

 * 内部类：因为这个临时的类是写在其他类里面的

 * 对象：匿名内部类 new 完之后会得到一个临时的子类对象

   使用前提 必须继承一个父类或实现一个接口
   
   使用一次之后就不能再使用，因为这只是临时拿出来用的
   
   使用匿名内部类的优点是可以少写一个CLASS文件。
   
   ```java
   父类类型 对象名 = new 父类类型() {
     @Override
     public void 方法名() {
       System.out.println("匿名内部类的临时实现...");
     }
   };
   ```
   
   ```java
   abstract class Person {
       public abstract void eat();
   }
    //		父类/接口 对象名 = new 父类/接口（）{
   	//		方法体
   //};
   public class Demo {
       public static void main(String[] args) {
           Person p = new Person() {
               public void eat() {
                   System.out.println("eat something");
               }
           };
           p.eat();
       }
   }
   ```
   
   
