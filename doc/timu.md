# 1 #

## Java ##

### <基础> ###

#### byte short int long boolean char float double 占字节数？ ####

| 类型    | 字节数 |
| ------- | ------ |
| byte    | 1      |
| short   | 2      |
| int     | 4      |
| long    | 8      |
| boolean | 1      |
| char    | 2      |
| float   | 4      |
| double  | 8      |

#### hashcode 和 equals 的关系 ####

两个对象equals的时候，hashCode不一定相等；但是hashCode相等的时候一定equals；在将对象插入到map或set的时候，需要重写hashCode。在对其中一个方法重写的时候，需要将两个方法都重写。

在不对方法重写的时候，hashCode方法返回对象在内存中的地址算出来的一个数值；equals返回两个对象的hashCode的值是否相等。

#### 深拷贝、浅拷贝区别 ####

深拷贝和浅拷贝最根本的区别在于是否真正获取一个对象的复制实体，而不是引用。

#### java 异常体系？RuntimeException Exception Error 的区别，举常见的例子。 ####

Error和Exception是Throwable接口的仅有的两个实现。

Error表示错误，一般有StackOverFlowError、OutOfMemeryError等错误。

Exception是异常，RunTimeException是Exception的一类，其中包括常见的NullPointerException、ArrayIndexOutOfBoundsException等异常。除了RunTimeException以外还有IOException，比如FileNotFound、EOFException等。

#### lambda 表达式中使用外部变量，为什么要 final？ ####

lambda本质上是以一种简单的方式对一个接口的方法进行实现，然后实现接口。所以对lambda进行调用，实际上是还是对另外的一个类的实例化对象中的方法进行调用。

在Java中方法调用是值传递的，所以在lambda表达式中对变量的操作都是基于原变量的副本，不会影响到原变量的值。

使用final修饰变量，保证这个值在lambda中不会被修改，只作为一个常量在方法中参与执行。

使用StirngBuilder

### <集合> ###

#### Collection 有什么子接口、有哪些具体的实现？ ####

![](https://img-blog.csdnimg.cn/20190717224652123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2phdmFlZV9nYW8=,size_16,color_FFFFFF,t_70)

Set、Queue、List。

HashSet、TreeSet、LinkedHashSet、LinkedBlockingQueue、ArrayBlockingQueue、LinkedList、ArrayList、Vector等实现。

#### 简单介绍下 ArrayList 怎么实现，add操作、get操作，什么时候扩容？ ####

arrayList底层使用一个数组来实现，add操作将数组的下一位设置成新传入的那个对象。get(int index)通过数组的index获取数组位上的对象返回即可。当数组的容量满了，还需要继续插入的时候，会对数组的容量进行扩容。

ArrayList的初始容量是10，每次扩容增加一半。

```java
int newCapacity = oldCapacity + (oldCapacity >> 1);
```

#### 讲一下 hashMap 原理。hashMap 可以并发读么？并发写会有什么问题？ ####

<略>

#### 讲一下 concurrentHashMap 原理。头插法还是尾插法？扩容怎么做？ ####

<略>

#### 堆是怎么存储的，插入是在哪里？ ####

<略>

#### 集合在迭代的过程中，插入或删除数据会怎样？ ####

会抛出 ConcurrentModificationException 异常。

通过对集合内部的 modCount 这个字段进行检查来实现的。

### <并发> ###

#### 进程和线程的区别？并行和并发的区别？了解协程么？ ####

根本区别：进程是操作系统资源分配的基本单位，而线程是任务调度和执行的基本单位

在开销方面：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

所处环境：在操作系统中能同时运行多个进程（程序）；而在同一个进程（程序）中有多个线程同时执行（通过CPU调度，在每个时间片中只有一个线程执行）

内存分配方面：系统在运行的时候会为每个进程分配不同的内存空间；而对线程而言，除了CPU外，系统不会为线程分配内存（线程所使用的资源来自其所属进程的资源），线程组之间只能共享资源。

包含关系：进程是线程的容器，如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

#### 进程 A 想读取进程 B 的主存怎么办？ ####

使用进程间通信。信号、信号量、消息队列、socket等。

#### 线程的生命周期有哪些状态？怎么转换？ ####

创建、就绪、运行中、等待、限时等待、阻塞、终止。

其中就绪和运行中称作可运行（runnable）。

创建 -> 就绪 -> 运行中 -> 终止

运行中 -> 等待 -> 就绪 -> 运行中

#### wait 和 sleep 有什么区别？什么情况下会用到 sleep？ ####

wait和sleep都会释放出cpu的资源，它们的区别在于wait会释放锁资源，而sleep不会释放锁的资源，直接进入阻塞状态。wait的苏醒需要同一个对象使用notify或notifyAll方法，而sleep到达指定的时间就能继续执行。

#### 怎么停止线程？ ####

使用中断，作为线程运行下去的判条件。

#### 怎么控制多个线程按序执行？ ####

使用 synchronized 关键字，配合wait、notify、notifyAll方法使用即可实现多个线程按照顺序执行。

#### 会用到线程池么？怎么使用的？用什么实现的？ ####

会用到线程池。

在批量执行任务的时候，比如想看项目清理不活跃用户的金币的时候。建立一个线程池批量执行执行任务，辅助以 RateLimiter 控制任务提交速度。

<略 - 线程池源码解析>

https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html

#### 常用的线程池有哪些？用的哪个线程池？什么情况下怎么选择？ ####

newFixedThreadPool：固定大小线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大值`nThreads`。线程池的大小一旦达到最大值后，再有新的任务提交时则放入无界阻塞队列中，等到有线程空闲时，再从队列中取出任务继续执行。

newSingleThreadExecutor：单线程化的线程池。它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序执行。

newScheduledThreadPool：固定大小的线程池。支持定时及周期性任务执行。

CachedThreadPool：可缓存线程池。如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。

#### ThreadPoolExecutor 有什么参数？各有什么作用？拒绝策略? ####

1. 核心池容量 
2. 最大线程数量
3. 拒绝策略
4. 线程工厂
5. 阻塞队列
6. 存活时间
7. 存活时间单位

#### 一个任务从被提交到被执行，线程池做了哪些工作？ ####

各种worker的操作，此处略。

### <锁> ###

#### 讲一下锁，有哪些锁，有什么区别，怎么实现的？ ####

synchronized、ReentrantLock。synchronized是jvm实现的，ReentrantLock是JDK实现的。在Synchronized优化以前，synchronized的性能是比ReenTrantLock差很多的，但是自从Synchronized引入了偏向锁，轻量级锁（自旋锁）后，两者的性能就差不多了，在两种方法都可用的情况下，官方甚至建议使用synchronized，其实synchronized的优化我感觉就借鉴了ReenTrantLock中的CAS技术。都是试图在用户态就把加锁问题解决，避免进入内核态的线程阻塞。

ReentrantLock基于AQS实现的。

#### ReentrantLock 应用场景？ ####



#### synchronized的实现原理 ####

monitor监视器：

monitorenter和monitorexit两个指令

#### 死锁条件 ####

四个，和操作系统的那几个一致。

#### 了解 AQS 么？讲讲底层实现原理。 ####



#### AQS 有那些实现？ ####



#### 讲讲 AtomicInteger 的底层实现。 ####



#### volatile 关键字有什么用？怎么理解可见性，一般什么场景去用可见性 ####



#### 讲一下 threadLocal 原理，threadLocal 是存在 jvm 内存哪一块的 ####



### <I/O> ###

IO 这块我不熟，没有多讲

了解 NIO 么？讲讲
NIO 与 BIO 有什么区别？
了解 Netty 原理么

### <虚拟机> ###

1）内存与 GC

jvm 内存区域分布？gc 发生在哪些部分？
介绍一下垃圾回收过程。
垃圾回收算法的了解。现在用的什么回收算法？
现在使用的什么垃圾回收器？知道哪些？讲讲 G1
容器的内存和 jvm 的内存有什么关系？参数怎么配置？
2）异常与调优

线上有什么 jvm 参数调整？
oom 问题排查思路
线上问题排查，突然长时间未响应，怎么排查，oom
cpu 使用率特别高，怎么排查？通用方法？定位代码？cpu高的原因？
频繁 GC 原因？什么时候触发 FGC？
怎么获取 dump 文件？怎么分析？
3）类加载器

怎么实现自己的类加载器？
类加载过程？
初始化顺序？

## Spring框架 ##

#### spring 介绍一下 ####

spring是一个轻量级的容器，spring是一个实现了IoC的容器，提供了对AOP的支持，提供了MVC web框架的实现，并且可以简单的整合现有的各种框架进行

讲一下 ioc、aop

ioc 怎么防止循环依

aop 的实现原理、动态代理过程

bean在哪个时间点注入到容器中

tomcat 与 spring、controller 的关系

spring boot starter 自加载是怎么实现的？在生命周期哪个阶段？

Spring 处理请求的过程？



## 数据库 ##

数据仓库与 mysql 区别？hive 和 mysql 有什么区别？spark 和 hadoop 区别？mapreduce 互相等待，怎么解决？
acid 含义？事务隔离级别？幻读怎么解决的？
用过 mysql 的锁么？有哪些锁？
MyISAM、InnoDB 区别？为什么不用 MyISAM？
mvcc 原理？多版本数据存放在哪？
mysql 脏页？
redo log，undo log？
索引
innodb 的索引结构是什么？什么是聚簇索引？
b+ 树与 b 树的区别？
b+ 树与二叉树区别，优点？为什么不用红黑树？
多列索引的结构
字符串类型和数字类型索引的效率？数据类型隐式转换
主键与普通索引的联系？存储上的区别？
sql
join 和 in 怎么选择？有什么区别？
union 和 union all 有什么区别？怎么选择？
怎么处理 sql 慢查询？
索引用得不太正常怎么处理？同时有（a，b）和（a，c）的索引，查询 a 的时候，会选哪个索引？
跨库分页的实现？
分库分表有哪些策略？怎么保证 id 唯一？
对 uuid 的理解？知道哪些 GUID、Random 算法？
主键选随机 id、uuid 还是自增 id？为什么？主键有序无序对数据库的影响？
主从复制的过程？复制原理？怎么保证强一致性？

### <网络> ###

tcp
tcp 有哪些机制确保可靠性？拥塞控制怎么实现？
close_wait 太多怎么处理？为什么会出现这种情况？
讲讲三次握手，四次挥手
http
http 2 有了解过么，新增了哪些功能，现在用的什么版本？1.1？
http 缓存机制都有哪些？什么是 cdn？header 中涉及到缓存的字段有哪些？
cookie session 介绍一下
html 页面，怎么与后端交互？流程是什么？涉及到哪些组件？
http 协议，报文格式？
keepalive 有什么用？
Https 原理？
知道哪些 http 状态码有哪些？
http 有哪些请求方法？put、post 实现上有什么区别？
前后端分离与不分离的区别？各有什么优缺点？
常见 web 攻击有哪些？了解 csrf 攻击么？
restful 的作用？有哪些优点和缺点？
nginx 达到上限了怎么办？怎么对 nginx 负载均衡？dns？
nginx 负载均衡有哪些算法？各自有什么优缺点？

### <redis数据库> ###

Redis 数据结构、对象，使用场景
Redis 内存淘汰策略
缓存的热点 Key 怎么处理？redis 缓存穿透，怎么避免？
redis keys 命令有什么缺点
主从同步原理，新加从库的过程
RDB 和 AOF 怎么选择，什么场景使用？
redis 的 zset 的使用场景？底层实现？为什么要用跳表？
怎么实现 redis 分布式锁？
3.7 Kafka

用 kafka 做了什么功能？
kafka 内部原理？工作流程？
Kafka 怎么保证数据可靠性？
怎么实现 Exactly-Once？
3.8 分布式

有哪些分布式组件是你最熟悉的，简单聊一聊。
cap 是指什么？mysql 满足 cap 中哪些？
分布式锁有哪些方式可以实现？各有什么优缺点？
什么是一致性 hash？自己实现一致性 hash，会用什么数据结构？
3.9 微服务

微服务用的什么体系？
讲一下熔断概念？熔断原理？令牌桶？熔断三个状态关系？
熔断会影响性能么？有遇到过线上发生熔断么？不加会怎样？
什么是 RPC？怎么实现幂等性？
微服务有什么优缺点？
配置中心有哪些选项？apollo 的架构？怎么无感实现已加载数据更新？
3.10 设计模式

工厂方法和抽象工厂的区别
装饰器和代理区别
单例
对于单例，你知道哪些实现方法？
实现一个懒加载单例
双重校验锁为什么需要双重校验？
