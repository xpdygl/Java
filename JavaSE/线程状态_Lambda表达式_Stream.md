## 线程

### 线程池

频繁的创建销毁线程会消耗系统过多资源，我们使用线程池让线程可以复用



线程池是一个容纳很多线程的容器，创建的线程就不会被销毁，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源。

+++



#### 线程池的好处



1. 降低资源消耗。减少了创建和销毁线程的次数，每个工作线程都可以被重复利用，可执行多个任务。
2. 提高响应速度。当任务到达时，任务可以不需要的等到线程创建就能立即执行。
3. 提高线程的可管理性。可以根据系统的承受能力，调整线程池中工作线线程的数目，防止因为消耗过多的内存，而把服务器累趴下(每个线程需要大约1MB内存，线程开的越多，消耗的内存也就越大，最后死机)。

创建线程池

```java
  ExecutorService es = Executors.newFixedThreadPool(3);
// 创建并提交任务
MyRunnable mr = new MyRunnable();
//Future<?> f = es.submit(mr);
//System.out.println(f.get());// null
es.submit(mr);
es.submit(mr);
es.submit(mr);
es.submit(mr);
es.submit(mr);
es.submit(mr);
    // 销毁线程池(一般不操作)
      es.shutdown();
```

真正的线程池接口是java.util.concurrent.ExecutorService。

### 线程状态

线程一共有六种状态

| 线程状态                | 导致状态发生条件                                             |
| ----------------------- | ------------------------------------------------------------ |
| NEW(新建)               | 线程刚被创建，但是并未启动。还没调用start方法。MyThread t = new MyThread()只有线程对象，没有线程特征。**创建线程对象时** |
| Runnable(可运行)        | 调用了 start() 方法，此时线程可能正在执行，也可能没有，这取决于操作系统的调度。**调用start方法时** |
| Blocked(锁阻塞)         | 当线程试图获取锁对象，而该锁对象被其他的线程持有，则该线程进入锁阻塞状态；当该线程获取到锁对象时，该线程将变成可运行状态。**等待锁对象时** |
| Waiting(无限等待)       | 一个线程在等待另一个线程执行一个（唤醒）动作时，该线程进入Waiting状态。进入这个状态后是不能自动唤醒的，必须等待另一个线程调用notify或者notifyAll方法才能够唤醒。**调用wait()方法时** |
| Timed Waiting(计时等待) | 同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。这一状态将一直保持到超时期满或者接收到唤醒通知。带有超时参数的常用方法有Thread.sleep 、Object.wait。**调用sleep()方法时** |
| Teminated(被终止)       | 因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。**run方法执行结束时** |



刚创建线程是就是新建状态，给线程Strat之后就是Runnable状态，

此时线程可能运行也可能没被运行

线程试图获得锁的时候，如果锁被别的线程拿走，那么此时处于锁阻塞状态，拿到锁就又回到Runnable状态。

当线程启用wait方法等待别的线程唤醒是就处于无限等待状态，别的线程不唤醒他就等一辈子。

当线程启用wait方法并指定等待的时间是时，就处于计时等待状态，超过这个时间还没有线程来唤醒，就会进入别的状态。拿到锁就是Runnable状态。没拿到锁就是锁阻塞状态

最后还有一个终止状态，当run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。

```java
public class Demo {
    public static Object Lock = new Object();
    public static boolean sign = false;

    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (Lock) {
                        if (!sign) {
                            try {
                                Lock.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }


                        if (sign) {
                            System.out.println("狂吃包");
                            try {
                                Thread.sleep(1000);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            Lock.notify();
                            sign = false;
                        }

                    }
                }
            }
        }).start();


        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (Lock) {

                        if (sign) {

                            try {

                                Lock.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                            if (!sign) {
                                try {
                                    System.out.println("在做包子");
                                    Thread.sleep(1000);
                                } catch (InterruptedException e) {
                                    e.printStackTrace();
                                }
                                Lock.notify();
                                sign = true;
                            }
                    }
                }
            }
        }).start();
    }
}
```



### Lambda的基本规则

Lambda表达式的使用步骤:
                1.判断是否是函数式接口
                2.如果是,就直接写上()->{}
                3.填充小括号中的内容-->和函数式接口抽象方法的形参列表一致(参数名可以不一致,其余都要一致)
                4.填充大括号中的内容-->函数式接口抽象方法实现的代码(抽象方法需要执行的代码)

**省略规则**

- Lambda表达式小括号中的参数类型可以省略不写
- 如果Lambda表达式小括号中只有一个参数,那么小括号也可以省略不写
- 如果Lambda表达式大括号中只有一条语句,那么大括号,return关键字,以及语句分号都可以省略(必须一起省略)

```java
 public static void main(String[] args) {
        // 创建并启动线程----Lambda标准格式
        new Thread(()->{
            System.out.println("Lambda表达式方式1...");
        }).start();

        // 创建并启动线程----Lambda省略格式
        new Thread(()-> System.out.println("Lambda表达式方式2...")).start();

        System.out.println("-------------------------");
        
        ArrayList<Integer> list = new ArrayList<>();
        list.add(500);
        list.add(300);
        list.add(400);
        list.add(100);
        list.add(200);
        System.out.println("排序之前:"+list);
        // 指定规则排序--->降序
        // Lambda标准格式
        // Collections.sort(list,(Integer i1,Integer i2)->{ return i2 - i1;});
        
        // Lambda省略格式
        Collections.sort(list,( i1, i2)-> i2 - i1);
        System.out.println("排序之后:"+list);
    }
```

## Stream流

```java

public class Test {
    public static void main(String[] args) {
        List<String> one = new ArrayList<>();
        one.add("迪丽热巴");
        one.add("宋远桥");
        one.add("苏星河");
        one.add("老子");
        one.add("庄子");
        one.add("张三丰");
        one.add("孙子");
        one.add("洪七公");
        //需求:
        //1. 队伍中只要名字为3个字的成员姓名；
        //2. 在名字为3个字的成员姓名中筛选出前3个人；
        // 获取流----->过滤出名字长度为3的姓名----------->取前3个----->打印输出元素
        one.stream().filter(name->name.length()==3).limit(3).forEach(name-> System.out.println(name));
    }
}
```

###  流操作思想概述

 可以将流操作思想类比成工厂车间的流水线\河流...

特点:

流一定要搭建好完整的函数模型,函数模型中必须要有终结方法

Stream流不能重复操作,也就是__一个Stream流只能使用一次__

Stream流不会存储数据的

Stream流不会修改数据源

### 获取流方式(Collection，Map)

 // 根据单列集合获取流
       __Stream<String> stream = col.stream();__



- 根据集合获取流---->Collection<E>集合中有一个获取流的方法`public default Stream<E> stream();`

  - 根据Collection获取流

    ```java
    /**
     * @Author PengZhiLin
     * @Date 2021/7/1 15:00
     */
    public class Test1_根据Collection获取流 {
        public static void main(String[] args) {
            // 创建Collection集合,限制集合元素类型为String
            Collection<String> col = new ArrayList<>();
            col.add("itheima");
            col.add("itcast");
            col.add("java");
    
            // 根据单列集合获取流
            Stream<String> stream = col.stream();// itheima,itcast,java
    
        }
    }
    
    ```

    

  - 根据Map获取流

    - 根据Map集合的键获取流

    - 根据Map集合的值获取流

    - 根据Map集合的键值对对象获取流

      ```java
      /**
       * @Author PengZhiLin
       * @Date 2021/7/1 15:04
       */
      public class Test2_根据Mao获取流 {
          public static void main(String[] args) {
              // 创建Map集合,限制键的类型为String,值的类型为String
              Map<String,String> map = new HashMap<>();
              map.put("王宝强","马蓉");
              map.put("贾乃亮","李小璐");
              map.put("陈羽凡","白百何");
              map.put("谢霆锋","张柏芝");
              
              //- 根据Map集合的键获取流
              // 获取锁的key
              Set<String> keys = map.keySet();
              Stream<String> stream1 = keys.stream();// 王宝强,贾乃亮,陈羽凡,谢霆锋
              
              //- 根据Map集合的值获取流
              Collection<String> values = map.values();
              Stream<String> stream2 = values.stream();// 马蓉,李小璐,白百何,张柏芝
              
              //- 根据Map集合的键值对对象获取流
              Set<Map.Entry<String, String>> entries = map.entrySet();
              // 键值对对象: 王宝强=马蓉,贾乃亮=李小璐,陈羽凡=白百何,谢霆锋=张柏芝
              Stream<Map.Entry<String, String>> stream3 = entries.stream();
          }
      }
      
      ```

      ### Stream流的相关操作

      能够掌握常用的流操作

      #### 终结方法

         __forEach：__逐一处理流中的元素

      __count：__统计流中元素的个数 (要打印出来)

      #### 非终结方法

      __filter:__

      __limit:__取流中前几个元素

      __skip:__ 跳过流中前几个元素

      __concat:__拼接两个流

      ```java
      				// 获取流
              Stream<String> stream1 = Stream.of("100", "200", "300", "400", "500");
              // 获取流
              Stream<String> stream2 = Stream.of("张三丰", "张无忌", "杨过", "郭靖");
              // 拼接2个流
              Stream<String> stream = Stream.concat(stream1, stream2);
              stream.forEach(s-> System.out.println(s));
      ```

      

      __map:__映射\转换__转换流中元素的类型__

      - `<R> Stream<R> map(Function<? super T,? extends R> mapper);转换流中元素的类型`
      - T,R的类型可以一致,也可以不一致
      - 哪个Stream流调用map方法,T就为该流中元素的类型
      - map方法返回的Stream流中元素的类型
      - 参数Function叫做转换接口,把T类型的元素转换为R类型的元素

      ```java
      /**
       * @Author PengZhiLin
       * @Date 2021/7/1 16:05
       */
      public class Test {
          public static void main(String[] args) {
              List<String> one = new ArrayList<>();
              one.add("迪丽热巴");
              one.add("宋远桥");
              one.add("苏星河");
              one.add("老子");
              one.add("庄子");
              one.add("孙子");
              one.add("洪七公");
      
              List<String> two = new ArrayList<>();
              two.add("古力娜扎");
              two.add("张无忌");
              two.add("张三丰");
              two.add("赵丽颖");
              two.add("张二狗");
              two.add("张天爱");
              two.add("张三");
      
              // 获取流--->操作流--->终结方法
              //1. 第一个队伍只要名字为3个字的成员姓名；
              //2. 第一个队伍筛选之后只要前3个人；
              Stream<String> stream1 = one.stream().filter((String t) -> {
                  return t.length() == 3;
              }).limit(3);
      
              //3. 第二个队伍只要姓张的成员姓名；
              //4. 第二个队伍筛选之后不要前2个人；
              Stream<String> stream2 = two.stream().filter((String t) -> {
                  return t.startsWith("张");
              }).skip(2);
      
              //5. 将两个队伍合并为一个队伍；
              //6. 根据姓名创建Person对象；
              //7. 打印整个队伍的Person对象信息。
              Stream.concat(stream1,stream2).map((String name)->{return new Person(name);}).forEach(p-> System.out.println(p));
          }
      }
      
      
      ```

      ### 将Stream流收集到集合中

      ```java
              // 获取流
              Stream<String> stream2 = Stream.of("张三丰", "张无忌", "杨过", "郭靖");
              // 把流中的元素收集到set集合
              Set<String> set = stream2.collect(Collectors.toSet());
              // [杨过, 张三丰, 郭靖, 张无忌]
      ```

      