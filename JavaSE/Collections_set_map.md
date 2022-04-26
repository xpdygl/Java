## Collections类

Collections的常用方法

```java
Collections.shuffle(List);										//打乱列表中元素的顺序
Collections.addall(List,1,2,3,4,5,6,);				//逗号后面可以加很多个元素，也可以加字符串，也可以加对象
Collections.sort(List);												//对列表进行排序
public static <T extends Comparable<? super T>> void sort(List<T> list)
- 集合元素是数值类型，排序规则是按元素的大小进行升序排序。
- 集合元素是字符串类型，排序规则是字典排序，即根据字符串中的字符在编码表中的编码值进行排序。
- 集合元素是自定义类型，排序规则是根据 Comparable 接口的 compareTo 方法进行排序。
  			compareTo 规则（前：this，后：参数 o）
  		  - 前减后：升序
  		  - 后减前：降序
```

Collections 工具类的 sort 方法有一个重载方法，支持根据自定义的排序规则对集合中的元素进行排序。

```java
//排序
        Collections.sort(list, new Comparator<Student>() {

            /**
             * 前：o1
             * 后：o2
             *
             * 前减后：升序
             * 后减前：降序
             */
            @Override
            public int compare(Student o1, Student o2) {
                return o1.getAge() - o2.getAge();
            }
        });

     System.out.println("排序后：" + list);
    }
```

## 可变参数

```java
权限修饰符 返回值类型 方法名(数据类型... 参数名) {
  //
}
public void Insert (int... num)
```

+ 1、一个方法只能有一个可变参数

+ 2、__如果方法有多个参数，可变参数必须定义在参数列表的最后__

这样就可以一直加同一类型的参数。

+++



## set

Set 是一个接口，继承自 Collection 接口，是单列集合的一个分支。

Set 集合的特点
    - 元素无索引
        - 元素不可重复

Set 集合的实现类
    - HashSet （底层结构是哈希表，存取无序）
        - LinkedHashSet （底层结构是链表 + 哈希表，存取有序）
        - TreeSet（底层结构是红黑树，存取无序，支持对元素进行排序

### HashSet

HashSet 是 Set 接口的实现类，它的底层由哈希表实现，特点是无索引、元素不可重复、存取无序。

```java
public static void main(String[] args) {
  //创建 HashSet 集合
  HashSet<String> set = new HashSet<>();
  set.add("cba");
  set.add("bac");
  set.add("cab");
  set.add("bca");
  set.add("abc");
  //打印集合中的元素
  System.out.println(set); 
  //[bca, cba, abc, bac, cab]
  //出现的顺序是随机的
}

```

HashSet 集合是如何保证元素唯一的呢？

使用 equals 判断是一个不错的方法，但每添加一个元素到集合中，都要调用 equals 和集合中所有的元素进行比较，效率太低。

Java 的解决访问是先判断待添加元素的 hashCode 在集合中是否已经存在，如果不存在，则说明当前添加的不是重复元素。但 hashCode 并非完全可靠，两个不同的对象，hashCode 却可能是相同的，例如字符串 Aa 和 BB 的 hashCode 就是相同的。所以，在 hashCode 相同的情况下，还需要再调用 equals 方法进一步确认，避免出现误判。

注意：添加到 HashSet 集合中的元素类型，必须要重写 hashCode 和 equals 方法！__（java有提供快捷键重写）__



java实现hashset元素不重复的原理是

1.先计算每个元素的哈希值

2.哈希值不同  元素就一定不相同

3.哈希值万一相同在用equals方法比较两个元素的值

这样就可以保证hashset里面的元素都是不重复的。

+ 需要注意的是一定要重写hashCode 和 equals 方法！



### LInkedSet

 HashSet 集合是存取无序的，如果我们希望存取有序，可以使用 HashSet 的子类 LinkedHashSet 集合，它的底层结构是链表+哈希表。

```java
public static void main(String[] args) {
  //创建 LinkedHashSet 集合
  LinkedHashSet<String> set = new LinkedHashSet<>();
  set.add("cba");
  set.add("bac");
  set.add("cab");
  set.add("bca");
  set.add("abc");
  //打印集合中的元素
  System.out.println(set); //[cba, bac, cab, bca, abc]
}

```



### TreeSet

TreeSet 集合是 Set 接口的一个实现类，底层是基于红黑树的实现。它的特点是没有索引、元素不可重复、存取无序，支持对集合中的元素进行排序。

```java
public TreeSet(Comparator<? super E> comparator)
```



注意：由于 TreeSet 需要对集合中的元素进行排序，所以元素类型必须实现 Comparable 接口并重写 compareTo 方法，或者指定自定义的比较器。



## Map

Map 接口中定义了双列集合的共有方法，Map 集合是一种__双列集合，元素以键值对__的形式存储在集合中。

单列集合与双列集合的区别
    - 单列集合：集合中的每一个元素只有一个值，或者是一个对象。
        - 双列集合：集合中的每一个元素是以 键值对 的形式存储的，即元素有键和值两个部分。





Map 集合的特点
    - Map 集合存储的元素是键值对
        - __Map 集合的键是唯一的，不可重复，但值可以重复__
        - 根据键取值



Map 接口的常用实现类
    - HashMap （底层采用数组+链表+红黑树结构，存取无序）
        - LinkedHashMap （在 HashMap 的基础上增加了链表结构来维护存取顺序，存取有序）
        - TreeMap （底层采用红黑树结构，存取无序，支持对集合元素进行排序）



Map常用方法

```java
Map.put(key,value);
Map.remove(keu,value)
Map.get(key)
Map.containskey
set<K> keyset()										//获得所有key的set集合
set<Map.Entry<K,V>> entryset()		//获得所有键值对的set集合
```

遍历map

```java
 //遍历map中的统计结果
        Set<Map.Entry<Character, Integer>> entries = map.entrySet();
        for (Map.Entry<Character, Integer> entry : entries) {
            Character key = entry.getKey();		//得到所有key
            Integer value = entry.getValue();	//得到所有value
            System.out.println(key + ": " + value);
        }
```

Map 集合的 key 不允许重复，依据的是 hashCode 和 equals 方法的判断结果。所以当 Map 的 key 是自定义类型时

__必须重写 hashCode 和 equals 方法。__







map的例子如下

```java
package com.itheima.demo12_TreeMap集合;

import java.util.Comparator;
import java.util.TreeMap;

/**
 * @author Qinghua Yang.
 */
public class Demo {


    public static void main(String[] args) {
        //数值类型：按照数值大小升序排序
        TreeMap<Integer, Integer> tm1 = new TreeMap<>();
        tm1.put(1, 1);
        tm1.put(3, 3);
        tm1.put(5, 5);
        tm1.put(2, 2);
        tm1.put(4, 4);

        System.out.println(tm1);

        //字符串类型：按照字典排序
        TreeMap<String, String> tm2 = new TreeMap<>();
        tm2.put("c", "c");
        tm2.put("e", "e");
        tm2.put("d", "d");
        tm2.put("a", "a");
        tm2.put("b", "b");

        System.out.println(tm2);

        //自定义类型（实现 Comparable）
        TreeMap<Student, String> tm3 = new TreeMap<>();
        Student stu1 = new Student("张三", 18);
        Student stu2 = new Student("李四", 20);
        Student stu3 = new Student("王五", 19);
        tm3.put(stu1, "张三");
        tm3.put(stu2, "李四");
        tm3.put(stu3, "王五");
        System.out.println(tm3);

        //自定义类型（比较器 Comparator）
        TreeMap<Person, String> tm4 = new TreeMap<>(new Comparator<Person>() {
            /**
             * 前：o1
             * 后：o2
             *
             * 前减后：升序
             * 后减前：降序
             */
            @Override
            public int compare(Person o1, Person o2) {
                return o2.getAge() - o1.getAge();
            }
        });

        Person p1 = new Person("张三", 18);
        Person p2 = new Person("李四", 20);
        Person p3 = new Person("王五", 19);
        tm4.put(p1, "张三");
        tm4.put(p2, "李四");
        tm4.put(p3, "王五");
        System.out.println(tm4);
    }
}

class Person {

    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

class Student implements Comparable<Student> {

    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    /**
     * 前：this
     * 后：o
     *
     * 前减后：升序
     * 后减前：降序
     */
    @Override
    public int compareTo(Student o) {
        return this.age - o.age;
    }
}

```

## HashMap



HashMap 基于数组+链表+红黑树



初始容量：16

负载因子：0.75

扩容阈值：容量*负载因子



扩容：每次扩容新容量都是旧容量的2倍。

1、根据key的hashcode计算桶的位置

2、如果桶中是空的，直接插入键值对

3、判断桶中的第一个元素是否冲突

4、如果桶中是一颗树结构，尝试将键值对插入到树中

5、如果桶中是一个链表结果，遍历链表中的所有键值对

6、在链表中添加完元素之后，需要判断链表长度是否大于树化阈值，如果大于，并且数组长度达到64，才进行树化

7、如果有冲突，替换值，返回旧的值

8、没有冲突，判断是否需要扩容

## 斗地主案例

```java
package mapTest;

import java.util.*;

/**
 * //创建 Map 集合，存储牌的编号和牌本身
 * //创建集合，存储花色
 * <p>
 * //创建集合，存储牌面值
 * <p>
 * //添加大小王
 * <p>
 * //组装牌
 * <p>
 * //创建三个集合，用于存储玩家得到的编号
 * <p>
 * //创建一个集合，用于存储底牌得到的编号
 * <p>
 * //获得所有的编号
 * <p>
 * //将Set转换成List
 * <p>
 * //洗牌，打乱编号的顺序
 * <p>
 * //发牌，发编号
 * <p>
 * //对玩家和底牌的牌编号进行排序
 * <p>
 * //创建三个集合，存储玩家最终得到的牌
 * <p>
 * //创建一个集合，存储底牌
 * <p>
 * //根据玩家和底牌得到的编号去Map中取对应的牌
 * <p>
 * //打印结果
 */
public class frightTheLandlord {
    public static void main(String[] args) {
        //创建map 储存牌面
        Map<Integer, String> pokerMap = new HashMap<>();
        //创建集合，存储花色
        ArrayList<String> colors = new ArrayList<>();
        Collections.addAll(colors, "♠", "♥", "♣", "♦");
        //创建集合，存储牌面值
        ArrayList<String> numbers = new ArrayList<>();
        Collections.addAll(numbers, "2", "A", "K", "Q", "J", "10", "9", "8", "7", "6", "5", "4", "3");
        //添加大小王
        pokerMap.put(1, "大王");
        pokerMap.put(2, "小王");
        //组装牌
        int id = 3;
        for (String number :numbers ) {
            for (String color : colors) {
                pokerMap.put(id, color + number);
                id++;
            }
        }
        System.out.println(pokerMap);

        //创建三个集合，用于存储玩家得到的编号
        ArrayList<Integer> List1 = new ArrayList<>();
        ArrayList<Integer> List2 = new ArrayList<>();
        ArrayList<Integer> List3 = new ArrayList<>();
        //创建一个集合，用于存储底牌得到的编号
        ArrayList<Integer> List4 = new ArrayList<>();
        //获得所有的编号
        Set<Integer> idSet = pokerMap.keySet();
        //将Set转换成List
        List<Integer> idList = new ArrayList<>(idSet);
        //洗牌，打乱编号的顺序
        Collections.shuffle(idList);


        //发牌，发编号
        for (int i = 0; i < idList.size(); i++) {
            if (i > 50) {
                List4.add(idList.get(i));
            } else if (i % 3 == 0) {
                List1.add(idList.get(i));
            } else if (i % 3 == 1) {
                List2.add(idList.get(i));
            } else {
                List3.add(idList.get(i));
            }
        }

//        System.out.println("列表");
//        System.out.println(List1);
//        System.out.println(List2);
//        System.out.println(List3);
//        System.out.println(List4);
        //对玩家和底牌的牌编号进行排序
        Collections.sort(List1);
        Collections.sort(List2);
        Collections.sort(List3);
        Collections.sort(List4);
        System.out.println("排序后");
        System.out.println(List1);
        System.out.println(List2);
        System.out.println(List3);
        System.out.println(List4);
//        //创建四个集合，存储玩家最终得到的牌
        List<String> play1 = new ArrayList<>();
        List<String> play2 = new ArrayList<>();
        List<String> play3 = new ArrayList<>();
        List<String> play4 = new ArrayList<>();
        System.out.println("转移到新的List后");



        for (Integer integer : List1) {
            play1.add(pokerMap.get(integer));
        }
        System.out.println("map");


        for (Integer integer : List2) {
            play2.add(pokerMap.get(integer));
        }


        for (Integer integer : List3) {
            play3.add(pokerMap.get(integer));
        }

        for (Integer integer : List4) {
            play4.add(pokerMap.get(integer));
        }


//        //根据玩家和底牌得到的编号去Map中取对应的牌

        //打印结果
        System.out.println("刘备"+play1);
        System.out.println("关羽"+play2);
        System.out.println("张飞"+play3);
        System.out.println("底牌"+play4);
    }
}

```

