# Java杂学记录

## 1. GC

> [记一次线上频繁GC的排查过程](https://blog.csdn.net/weixin_42392874/article/details/89483496)
>
> [[JVM\]一次线上频繁GC的问题解决](https://www.cnblogs.com/zhengwangzw/p/10493562.html)
>
> [内存频繁GC问题查找分享](https://blog.csdn.net/u011983389/article/details/11126963)
>
> [Java 如何排查堆外内存使用情况？](https://www.v2ex.com/t/618931)
>
> [Java 对象内存布局](https://www.v2ex.com/t/356007)
>
> [Java 系统内存泄漏定位](https://www.v2ex.com/t/607922)
>
> [[分享] Java 对象内存布局](https://www.v2ex.com/t/356007)
>
> [Java 系统内存泄漏定位](https://www.v2ex.com/t/607922)
>
> [java内存dump文件导出与查看](https://blog.csdn.net/lsh2366254/article/details/84911374)
>
> [记一次频繁gc排查过程](https://www.jianshu.com/p/13915f4a562b)
>
> [[jvm\][面试]JVM 调优总结](https://www.cnblogs.com/diegodu/p/9849611.html)
>
> [深入浅出JVM调优，看完你就懂](https://blog.csdn.net/Javazhoumou/article/details/99298624)
>
> [static类/变量会不会被GC回收？](https://bbs.csdn.net/topics/80471342)
>
> [研究了 2 天，终于知道 JDK 8 默认 GC 收集器了！](https://blog.csdn.net/youanyyou/article/details/106464291?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

### 1. static的GC问题

>  [static类/变量会不会被GC回收？](https://bbs.csdn.net/topics/80471342)	

​	static变量一直都有指向，比如static int a = 999;那么一直有个a 引用 数值999，这里的a和数值999所占用的内存不会被回收。

​	如果static int[] a = new int[1024];然后之后又有 a = null;的操作，那么这个a所指向的int[1024]是以后会被GC的。

## 2. 动态代理

> spring默认使用jdk动态代理，如果类没有接口，则使用cglib。
>
> Spring5.X还是默认使用JDK动态代理，而SpringBoot2.X为了避免用户@Autowired使用不规范导致JDK动态代理失败，默认使用cglib
>
> [Java两种动态代理JDK动态代理和CGLIB动态代理](https://blog.csdn.net/flyfeifei66/article/details/81481222)
>
> [JDK和CGLIB动态代理区别](https://blog.csdn.net/yhl_jxy/article/details/80635012)
>
> [CGLib动态代理](https://www.cnblogs.com/wyq1995/p/10945034.html)
>
> [CGLIB(Code Generation Library) 介绍与原理](https://www.runoob.com/w3cnote/cglibcode-generation-library-intro.html)
>
> [ASM](https://www.jianshu.com/p/a1e6b3abd789)
>
> [秒懂Java动态编程（Javassist研究）](https://blog.csdn.net/ShuSheng0007/article/details/81269295)
>
> [什么鬼？弃用JDK动态代理，Spring5 默认使用 CGLIB 了？](https://blog.csdn.net/weixin_43167418/article/details/103900670)
>
> [JDK动态代理](https://www.cnblogs.com/zuidongfeng/p/8735241.html)

@Autowired

UserService userService； // JDK or CgLib都ok

@Autowired

UserServiceImpl userService; // JDK报错，因为该类型不是接口，JDK是代理是创建了接口类的一个实现类。CgLib则是根据继承实现的。

## 3. final

> 性能影响：总之就是影响甚微，没必要通过考虑使用final来"改变性能"。
>
> final只作用于编译阶段，运行阶段根本不存在final关键字更别说什么影响了，顶多就是编译过程中不同虚拟机对含有final的代码的解释方式不同，进而可能有不同的执行逻辑。但本质上，final在运行时没有作为。
>
> [Final关键字之于Java性能的影响](https://www.jianshu.com/p/e50029ec0ea7)
>
> [java方法中变量用final修饰对性能有影响！你觉得呢？](https://zhidao.baidu.com/question/459147671.html)
>
> [为什么一些人喜欢在java代码中能加final的变量都加上final](https://blog.csdn.net/qq_31433709/article/details/87823478)
>
> 

​	如果试图通过多用final来提升性能啥的，那还是算了。提升性能应该在算法使用、设计模式、业务逻辑等方面下功夫，而不是咬文嚼字地钻牛角尖。

## 4. 并发

> [Java并发——Executor框架详解（Executor框架结构与框架成员）](https://blog.csdn.net/tongdanping/article/details/79604637)
>
> [java并发编程：Executor、Executors、ExecutorService](https://blog.csdn.net/weixin_40304387/article/details/80508236)
>
> [Java中的线程池——ThreadPoolExecutor的使用](https://blog.csdn.net/u010723709/article/details/50377543)
>
> [线程池之ThreadPoolExecutor使用](https://www.jianshu.com/p/f030aa5d7a28)
>
> maxnumPoolSize大于corePoolSize的部分，仅在等待队列workQueue满时生效。
>
> （下述场景是使用上面文章中的代码，我自己经过测试的。我注释掉了“预启动所有核心线程”的代码。）
>
> 比如corePoolSize = 2，maxnumPoolSize = 4，workQueue容量2：
>
> + 同时提交3个Runnable，那么前两个会新建线程（这之后达到corePoolSize）；要是前两个线程执行慢，那么第三个线程会先在阻塞队列等待corePool空出来，而不会直接新建线程。
> + 同时提交6个Runnable，那么前两个task马上新建两个对应的县城，而第三个和第四个正好填满workQueue，第五个和第六个来的时候workQueue满了，且maxnumPoolSize>corePoolSize，所以第五个第六个直接新建两个对应的线程并执行，最后才是第三个和第四个等有线程空闲了，才被执行。
> + 同时提交7个Runnable，前两个还是马上新建线程，然后还是第五第六又新建线程执行，而中间的第三第四还在workQueue中，等待线程空闲时被执行，由于线程达到maxnumPoolSize，且workQueue满了，所以第七个Runnable直接被拒绝执行。
>
> [ThreadPoolExecutor使用详解](https://www.cnblogs.com/zedosu/p/6665306.html)
>
> 1.当线程池小于corePoolSize时，新提交任务将创建一个新线程执行任务，即使此时线程池中存在空闲线程。 
> 2.当线程池达到corePoolSize时，新提交任务将被放入workQueue中，等待线程池中任务调度执行 
> 3.当workQueue已满，且maximumPoolSize>corePoolSize时，新提交任务会创建新线程执行任务 
> 4.当提交任务数超过maximumPoolSize时，新提交任务由RejectedExecutionHandler处理 
> 5.当线程池中超过corePoolSize线程，空闲时间达到keepAliveTime时，关闭空闲线程 
> 6.当设置allowCoreThreadTimeOut(true)时，线程池中corePoolSize线程空闲时间达到keepAliveTime也将关闭** 
>
> 
>
> 锁
>
> [使用synchronized同步对象却有多个线程能同时访问，使用lock锁却达到目的了，不知道为什么求大神回](https://bbs.csdn.net/topics/391087139?page=1)
>
> 
>
> 并发异常
>
> [Java并发--ConcurrentModificationException（并发修改异常）异常原因和解决方法](https://www.cnblogs.com/bsjl/p/7676209.html)	<=	
>
> 
>
> 并发容器
>
> [Java并发--同步容器](https://www.cnblogs.com/bsjl/p/7660333.html)

## 5. 布隆过滤器

> [布隆过滤器介绍和应用](https://www.jianshu.com/p/03c8dad08035)

## 6. 内存分析

> [性能优化工具（八）-MAT](https://www.jianshu.com/p/97251691af88)
>
> [Java内存分析工具--IDEA的JProfiler和JMeter插件](https://blog.csdn.net/qq_19674905/article/details/80824858)
>
> [MAT使用笔记](https://blog.csdn.net/zgmzyr/article/details/8232323)

## 7. 浅拷贝和深拷贝

> [Java中的clone方法-理解浅拷贝和深拷贝](https://www.cnblogs.com/JamesWang1993/p/8526104.html)

## 8. 类型擦除

> [Java类型擦除机制](https://www.cnblogs.com/chenpi/p/5508177.html)
>
> 范型仅在编译时有效，运行时都是Object。范型不具备继承关系。
>
> 通配符"?" => 编译时不知道类型，所以只能get不能add；
>
> 有界通配符"\<? extends Object\>","\<? super Object\>"可以add对应Object或其子类/父类。（因为我们编译前给定了父类/子类）
>
> [**谈谈java泛型的擦除机制**](http://www.zuidaima.com/blog/3689434434948096.htm)

## 9. 编码问题

> [几种常见的编码格式 ](https://www.cnblogs.com/mlan/p/7823375.html)
>
> [Java中char到底是多少字节？](https://www.iteye.com/topic/47740)

## 10. 容器

> [Java HashMap 原理（位桶 + 链表）](https://blog.csdn.net/wilson1068/article/details/88142495) <= 推荐阅读
>
> JDK 1.6, JDK 1.7 HashMap 采用位桶 + 链表实现。
>
> JDK 1.8 HashMap 采用位桶 + 链表 + 红黑树实现。（当链表长度超过阈值 “8” 时，将链表转换为红黑树）。
>
> HashMap 容量 (capacity) 和负载因子 (loadFactor)。
>
> capacity初始值16，loadFactor为0.75。index = hash mod 2<sup>n</sup>,初始n=4。
>
> 当数组中的节点（**entry**）数目 &gt; capacity \* loadFactor \> capacity \* loadFactor \> capacity \* loadFactor时，就需要扩容，调整数组的大小为当前的 2 倍，以提高 HashMap 的 hash 效率。
>
> [Java中的Iterable与Iterator详解](https://www.cnblogs.com/litexy/p/9744241.html)

## 11. 设计模式

> [JAVA设计模式之观察者模式](https://www.cnblogs.com/luohanguo/p/7825656.html)

## 12. Guava

> [Guava Cache用法介绍](https://www.cnblogs.com/fnlingnzb-learner/p/11022152.html) <== 极力推荐，包括中间提及的[使用缓存的9大误区（上）](https://kb.cnblogs.com/page/138696/)和[使用缓存的9大误区（下）](https://kb.cnblogs.com/page/144396/)

## 13. 字符编码

> [字符编码笔记：ASCII，Unicode 和 UTF-8](https://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
>
> windows采用GBK，Linux通常UTF-8（变长编码，Unicode的一种。现在Unicode通常指UTF-16，定长2字节）。
>
> 表示顺序（大端和小端=> java大端，c语言小端）

## 14. Disruptor

> [高性能队列——Disruptor](https://tech.meituan.com/2016/11/18/disruptor.html)
>
> [伪共享](https://zhuanlan.zhihu.com/p/124974025)
>
> [Java8的伪共享和缓存行填充--@Contended注释](https://www.cnblogs.com/Binhua-Liu/p/5623089.html)
>
> Java8中提供了官方的解决方案，Java8中新增了一个注解：@sun.misc.Contended。加上这个注解的类会自动补齐缓存行，需要注意的是此注解默认是无效的，需要在jvm启动时设置-XX:-RestrictContended才会生效
>
> 上面这篇文章中，我试了他代码，我是同组@Contended耗时大概是不同组的1/6。（不同组速度慢是因为线程t0和t1互相使对方的缓存行失效了。）

@Contended的测试代码来自上述文章（jdk8，64位，CPU环境2.4GHz 8核i9，内存32G）

```java
package coding;

import sun.misc.Contended;

public final class FalseSharing implements Runnable {
  public final static long ITERATIONS = 500L * 1000L * 1000L;
  private static VolatileLong volatileLong;
  private String groupId;

  public FalseSharing(String groupId) {
    this.groupId = groupId;

  }

  public static void main(final String[] args) throws Exception {
    // Thread.sleep(10000);
    System.out.println("starting....");

    volatileLong = new VolatileLong();
    final long start = System.nanoTime();
    runTest();
    System.out.println("duration = " + (System.nanoTime() - start));
  }

  private static void runTest() throws InterruptedException {
    Thread t0 = new Thread(new FalseSharing("t0"));
    Thread t1 = new Thread(new FalseSharing("t1"));
    t0.start();
    t1.start();
    t0.join();
    t1.join();
  }

  @Override
  public void run() {
    long i = ITERATIONS + 1;
    if (groupId.equals("t0")) {
      while (0 != --i) {
        volatileLong.value1 = i+1;
        volatileLong.value3 = i-1;
      }
    } else if (groupId.equals("t1")) {
      while (0 != --i) {
        volatileLong.value2 = i+1;
        volatileLong.value4 = i-1;
      }
    }
  }
}

class VolatileLong {
  @Contended("group0")
  public volatile long value1 = 0L;
  @Contended("group0")
  public volatile long value2 = 0L;

  @Contended("group1")
  public volatile long value3 = 0L;
  @Contended("group1")
  public volatile long value4 = 0L;
}
```

输出如下：

```none
//run函数分别是 13、24 （即不同组）
duration = 30464121663
duration = 30863580223
duration = 30235774560

//run函数分别是 12、34 （即同组）
duration = 5263218292
duration = 5322789201
duration = 5342706955
```

## 15. 中断

> [JAVA多线程之中断机制(如何处理中断？)](https://www.cnblogs.com/hapjin/p/5450779.html)
>
> [并发基础（八） java线程的中断机制](https://www.cnblogs.com/jinggod/p/8486096.html)



