

## 线程

### 1.线程与进程

> 进程是操作系统运行程序的基本单位，线程是进程的基本单位，一个进程可以有多个线程。在操作系统中以进程为单位分配资源，线程独享线程栈，寄存器环境，线程本地存储。

**进程**（process）：是计算机中的程序关于某数据集合上的一次运行活动，是操作系统进行资源分配与调度的基本								单位。简单来说就是操作系统中的运行的一个程序。

**线程**（thread） ：是进程的一个执行单元，一个线程就是进程中一个单一顺序的控制流。

**主线程与子线程**：JVM启动时会创建一个主线程，该线程执行main方法。父子线程是相对的，如果在A线程中创建							  了一个B线程，那么A线程就是B线程的父线程。





### 2.串行并发与并行

  ![Concurrent](D:\Git\BLOGS\docs\picture\javaProcess.png)

并发可以提高事务的效率



### 3.线程创建方式

java中创建线程有三种方式

#### 3.1 继承Thread类

``` java
package zom.zsf.java.io;

/**
 * @program: JavaBasic
 * @description:
 * @author: Zhang Shangfeng
 * @create: 2020-09-15 11:36
 **/
public class MyThread extends Thread {

    public static void main(String[] args) {
        Thread myThread = new MyThread();
        myThread.start();
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "---->创建了一个线程");
    }

}
```

调用线程的start启动线程，但并不意味着线程开始运行，线程的运行要执行run方法，线程具体的运行顺序由线程调度器决定



#### 3.2 实现 Runnable接口

``` java
package zom.zsf.java.io;

/**
 * @program: JavaBasic
 * @description:
 * @author: Zhang Shangfeng
 * @create: 2020-09-15 11:36
 **/
public class MyThread implements Runnable {

    public static void main(String[] args) {
        Thread myThread = new Thread(new MyThread());
        Thread myThread2=new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("以匿名内部类方式创建线程");
            }
        });
        myThread.start();
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "---->创建了一个线程");
    }

}

```

#### 3.3 JUC 实现Callable接口，

带返回值的线程，返回一个Future对象





### 4.线程常用方法

#### 4.1 currentThread 

​		Thread.currentThread()获取当前线程

#### 4.2 setName/getName	

​		设置/返回线程名

#### 4.3 isAlive

​		判断线程活动状态

#### 4.4 sleep

​		线程休眠

#### 4.5 getId

​		获取线程唯一标识

#### 4.6 yelid

​		放弃当前的cpu资源，使线程回到就绪状态

#### 4.7  setPriority

​		设置线程优先级 1-10

#### 4.8 interrupt

​		中断线程

#### 4.9 setDaemon

​		设置守护线程

​		守护线程是为其他线程服务的线程（例如GC），守护线程不能单独运行，当JVM中只剩下守护线程时，守护线程会自动销毁



### 5.线程安全

多线程的优势：

- 吞吐量
- 响应量
- 多核处理器利用效率



多线程的安全问题：

- 数据一致性问题

- 线程活性问题：由于程序自身的缺陷或者资源的稀缺性，使线程一直处于非RUNNABLE状态
  - 死锁 资源的争抢与占用
  - 锁死 没有这个资源
  - 活锁 资源的谦让
  - 饥饿 线程优先级引起的资源抢占  
- 上下文切换 从一个线程切换到另一个线程可能出现问题
- 可靠性 线程终止可能影响其他线程

#### 5.1 并发特性

- 原子性（Atomic）线程操作不可分割
  - 原子性有两层含义
    - 对于一个线程的操作，要么成功，要么失败。
    - 对于共享变量的操作是不能交替执行的
  - 使用锁实现  锁具有排他性，能够保证共享变量在某一时刻是线程私有的
  - 使用CAS（Compare and Swap）指令 在硬件层面保证原子性
- 可见性
  - 线程能时时刻刻看到共享变量的修改
- 有序性
  - 有序性指操作的执行顺序
    - 在多核处理器环境下，编写代码所指定的执行顺序是得不到保障的
    - 编译器可能改变代码执行顺序
    - 处理器可能不会按代码顺序执行
    - 一个处理器上的代码执行顺序在其他处理器来看与目标代码所定义的顺序不一致，这种现象称为**重排序**
      - 重排序是处理器为了提高程序执行效率而做的程序优化，他不保证指令执行顺序与代码一致，但是保证执行结果与目标代码相同
      - 重排序提高了性能，但可能引起正确性问题
  - 存储子系统
    - 写缓冲器（Store buffer，Write buffer）
    - 高速缓存（Cache）

#### 5.2 线程同步机制

##### 5.2.1 锁机制

​		利用锁机制将多个线程并发访问转化为串行访问

- 内置锁 synchronized关键字实现
  - java中每个对象都有一个与之关联的内置锁，也称监视器，这种内置锁是一种排它锁，可以保障原子性、可见性和有序性
  - synchronized关键字
    - 类锁和对象锁互不干扰
    - this 当前对象为锁对象，锁对象不同不能同步
    - 常量 不建议使用常量作为锁对象
    - 实例方法  同步代码块为整个方法，锁对象为this对象
    - 静态方法 默认运行时类作为锁对象，所有对象共享锁 **synchronized(Object.class)**
  - 同步方法和同步代码块的选择
    - 同步方法锁的粒度粗，执行效率低
    - 同步代码块锁的粒度细，执行效率高
- 显示锁 JUC Lock接口实现
  - 在JDK5中增加了Lock接口
  - ReentrantLock 可重入锁，理解为套娃
    - lock.lock()
    - lock.unlock()
    - 通常在try代码块中获得锁，在finally子句中释放锁
    - lockInterruptibly 如果当前线程未被中断则获得锁，若线程被中断则出现异常，在等待锁的过程中取消对锁的请求，能解决死锁问题
    - tryLock() 约定请求锁的时间，超时放弃请求锁；tryLock只会请求未被占用的锁
    - newCondition() 返回一个condition对象，实现等待/通知模式
      - await 使当前线程等待并释放锁，等待其他线程调用signal时重新执行
      - signal 唤醒一个等待线程，持有等待线程的对象锁才能唤醒
    - ReentrantLock无参构造默认为非公平锁（高效），有参为公平锁（先到先得）
    - getHoldCount 线程调用锁对象的次数
    - getQueueLength 正在等待的线程预估数
    - getWaitQueueLength(Condition condition) 与condition相关的等待线程预估数
    - hasQueuedThread 查询参数线程是否在等待获得锁
    - hasQueuedThreads  查询是否有线程在等待获得锁
    - hasWaiters 查询是否有正在等待的condition条件
    - isHeldByCurrentThread 判断锁是否被当前线程持有
    - isLocked 查询锁是否被线程持有
- 锁的相关概念
  - 可重入性 一个线程持有该锁时能够再次申请该锁
  - 争用与调度 内置锁属于**非公平锁**，显示锁**既支持非公平锁**，**又支持公平锁**。公平锁按照时间顺序，遵循先到先得的规则，不会引起线程饥饿，但公平锁的实现必须维护一个有序队列，成本较高，性能低，一般情况下不用
  - 锁的粒度 一个锁能够保护的共享资源的数量大小 。粒度过粗，会在申请时造成不必要的等待；粒度过细，会增加锁调度的开销

##### 5.2.2 volatile

volatile关键的作用在于使多个变量**可见**

volatile不具备原子性

##### 5.2.3 CAS

> CAS (Compare And Swap)是有硬件实现的，能够将读写修改（read、write、modify）操作转换为原子操作

为什么基本类型自增不能保证原子性：

i++自增操作包括三个子操作

1. 从主存读取i变量
2. i=i+1
3. 将修改后的值保存到主存



- CAS的原理：更新后再次读取主内存中的值，如果变量值等于期望值就更新

- ABA问题：
  - CAS操作中假设变量（修改后）当前值与期望值相同时，就视为此变量未被其他线程修改过。	
  - 而实际中共享变量可能经历了A->B->A的过程，是否能接受这种变化与实现的算法有关。
  - 规避ABA问题的办法：可以为共享变量添加一个修订号，每次修改共享变量时，相应的修订号+1，AtomicStampedReference就是基于此原理实现的

- 原子变量类
  - 基础数据型：AtomicInteger，AtomicLong，AtomicBoolean
  - 数组型：AtomicIntegerArray，AtomicLongArray，AtomicReferenceArray
  - 字段更新器：AtomicIntegerFieldUpdater，AtomicLongFieldUpdater，AtomicReferenceFieldUpdater
  - 引用型：AtomicReference,AtomicStampedReference，AtomicMarkableReferencw

#### 5.3 ReentrantReadWriteLock读写锁

synchronized与ReentrantLock锁都是独占锁（排它锁），同一时间只允许一个线程执行同步代码块，可以保证线程安全线，但效率低。

ReentrantReadWriterLock是一种改进的排它锁，也可以称为共享锁，允许多个线程同时读取共享数据，但是一次只允许一个线程对共享数据进行更新。读取数据前需要获取读锁，修改数据需要获取写锁。读锁可以被多个线程共享，写锁是排他的

- ReadWriteLock接口
  - readLock 返回读锁
  - writeLock 返回写锁
  - 读写锁是同一个锁的两个角色，是**同一个锁**
  - 读读共享、写写互斥、读写互斥

### 6. 线程间通信

#### 6.1 等待/通知机制

​	在单线程编程中，要执行的操作需要满足多个条件，可以把条件放在if语句中。

​	在多线程编程中，可能A线程需要的条件暂时不能满足，稍后B线程会提供这个条件，此时可以让A线程进行等待，直到B线程提供这个条件

- wait
  - 由对象调用而不是由线程调用
  - 只能在同步代码块中使用
  - 调用时会释放锁
  - 不是锁对象调用会产生异常
  - wait(long) 带有超时判断的等待
- notify
  - 需要等同步代码块运行完成后才释放锁对象，所以一般放在最后
- notifyAll
  - 唤醒所有线程
- interrupt
  - 中断等待

#### 6.2 生产者消费者模式

- 操作值
  - 一个生产者对应一个消费者，生产和消费交替执行
  - 一个生产者对应多个消费者，生产者来不及生产  if->while
  - 多个生产者对应多个消费者，消费者不一定唤醒生产者 notify->notifyAll

- 操作栈

#### 6.3 通过管道实现线程间通信

java.io包中的PipeStream管道流用于在线程间传送数据



- - 

### 7. 线程管理

#### 7.1 线程组

​		类似于计算机中的文件夹。Thread类中有几个构造方法允许在创建线程时指定线程组，默认创建线程时的线程组为父线程所在的线程组。JVM在创建main线程时会为其指定一个线程组，因此java中每个线程都有一个线程组与其关联，使用getThreadGroup方法返回。

​		线程组是出于安全考虑而设计的，但并未实现这一目标，现在一般将相关线程存储在数组或集合中，如果仅仅是为了区分，可以使用线程名实现，大多数情况可以忽略线程组

- 创建线程组

``` java
        //返回当前线程的线程组
        ThreadGroup threadGroup = Thread.currentThread().getThreadGroup();
        System.out.println(threadGroup);

        //定义线程组，如果不指定所属线程组，则自动归属为当前线程所属的线程组
        ThreadGroup threadGroup1 = new ThreadGroup("group1");
        System.out.println(threadGroup1);

        //定义线程组，同时指定父线程组,getParent返回父线程组
        ThreadGroup threadGroup2 = new ThreadGroup(threadGroup, "group2");
        System.out.println(threadGroup1.getParent() == threadGroup);
        System.out.println(threadGroup2.getParent() == threadGroup);

        //创建线程时指定所属的线程组
        Runnable runnable = () -> System.out.println(Thread.currentThread());
        //创建线程时不指定线程组则自动归属到父线程的线程组中
        //在main线程中创建t1线程，则main线程为父线程
        Thread thread = new Thread(runnable, "t1");
        System.out.println(thread);

        //创建线程时，可以指定线程所属线程组
        Thread thread1=new Thread(threadGroup1,runnable,"t2");
        Thread thread2=new Thread(threadGroup2,runnable,"t3");
        System.out.println(thread1);
        System.out.println(thread2);
```



- 基本操作

  - activeCount 返回当前线程组及子线程组中活动线程的数量（近似值）
  - activeGroupCount 返回当前线程组以及子线程组中活动线程组的数量（近似值）
  - enumerate 线程组的复制
    - Thread[] list 将当前线程组中活动线程复制到参数线程中
      - boolean recurse  false只复制当前线程组中的线程，不复制子线程组中的线程
    - ThreadGroup[] list 将当前线程组中的活动线程组复制到参数线程组中
  - getMaxPriority 返回线程组的最大优先级，默认是10
  - getName 返回线程组的名称
  - getParent 返回父线程组
  - intertupt 中断线程组中所有线程
  - isDaemon 判断线程组是否为守护线程组
  - list 将当前线程组中的活动线程打印出来
  - parentOf 判断当前线程组是否为参数线程组的父线程组
  - setDeamon 设置线程组为守护线程组

  ``` java
          ThreadGroup mainGroup = Thread.currentThread().getThreadGroup();
          //默认group的父线程组为mainGroup
          ThreadGroup group = new ThreadGroup("group");
          Runnable runnable = new Runnable() {
              @Override
              public void run() {
                  while (true) {
                      System.out.println("==========当前线程：" + Thread.currentThread());
                      try {
                          Thread.sleep(1000);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                  }
              }
          };
  
  
          Thread t1 = new Thread(runnable, "t1");
          Thread t2 = new Thread(group, runnable, "t2");
          t1.start();
          t2.start();
  
  
          //线程组的相关属性
          System.out.println("main线程组中活动线程数量"+mainGroup.activeCount());
          System.out.println("group线程组中活动线程数量"+group.activeCount());
          System.out.println("main线程组中子线程组数量"+mainGroup.activeGroupCount());
          System.out.println("group线程组中子线程组数量"+group.activeGroupCount());
          System.out.println("main线程组的父线程组"+mainGroup.getParent());
          System.out.println("group线程组的父线程组"+group.getParent());
          System.out.println(mainGroup.parentOf(mainGroup));
          System.out.println(mainGroup.parentOf(group));
          mainGroup.list();
  ```

  

#### 7.2 异常

在run方法中，如果有受检异常必须进行捕获处理，如果想获得run方法中出现的运行时异常，可以通过回调UncaughtExceptionHandler接口获得那个线程出现了运行时异常。

- getDefaultUncaughtExceptionHandler 获得全局异常UncaughtExceptionHandler
- getUncaugthExceptionHandler 获得当前UncaughtExceptionHandler
- setDefaultUncaughtException 设置线程全局UncaughtExceptionHandler
- setUncaughtExceptionHandler 设置当前线程的UncaughtExceptionHandler

``` java
        //设置线程全局回调接口
        Thread.setDefaultUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
            @Override
            public void uncaughtException(Thread t, Throwable e) {
                System.out.println(t.getName() + "线程产生了异常：" + e.getMessage());
            }
        });

        Thread thread = new Thread(() -> {
            System.out.println(Thread.currentThread().getName());
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(12 / 0);
        });
        thread.start();

        new Thread(() -> {
            String txt = null;
            System.out.println(txt.length());
        }).start();

    }
```

#### 7.3 注入Hook钩子线程

Mysql、Zookeeper、kafka等都存在Hook线程机制，校验线程是否已经启动，防止重复启动程序。

​		Hook线程也称钩子线程，当JVM退出时会执行Hook线程，经常在程序启动时创建一个.lock文件，用来校验程序是否启动，在程序退出时删除这个文件。还可以做资源释放，但是避免在Hook线程中进行复杂的操作。

``` java
    public static void main(String[] args) {
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("JVM退出，会启动当前Hook线程，在Hook线程中删除.lock文件");
            getLockFile().toFile().delete();
        }));

        if (getLockFile().toFile().exists()) {
            throw new RuntimeException("程序已启动");
        } else {
            try {
                getLockFile().toFile().createNewFile();
                System.out.println("程序在启动时创建了lock文件");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        for (int i=0;i<100;i++){
            System.out.println("程序正在运行");
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private static Path getLockFile() {
        return Paths.get("", "tmp.lock");
    }
```



### 8.线程池

#### 8.1 什么是线程池

​		当程序中的线程数量非常多，如果不对线程进行控制与管理，反而会耗尽CPU资源，影响程序的性能。

线程开销主要包括：

- 创建与启动线程的开销

- 线程销毁的开销

- 线程调度的开销

- 线程数量受限于CPU处理器数量

  ​	线程池就是线程高效利用的一种常用方式，线程池可以预先创建一定数量的工作线程，

客户端直接将任务作为一个对象提交给线程池，线程池将任务缓存在工作队列中，由创建好的

工作线程不断的从队列中取出并执行





#### 8.2 JDK线程池

​		JDK提供了一套Executor框架帮助开发人员有效使用线程池

​		线程池的简单使用：

``` java
        //创建有5个线程大小的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        //向线程池中提交18个任务
        for (int i=0;i<18;i++){
            executorService.execute(() -> {
                System.out.println(Thread.currentThread().getId()+"编号的任务正在执行，开始时间"+System.currentTimeMillis());
                try {
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
```

​		线程池的计划任务：

``` java
        //创建一个有调度功能的线程池
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(10);
        //在延迟2s后执行任务
        //schedule(Runnable,延迟时长，时间单位)
        scheduledExecutorService.schedule(() -> System.out.println(Thread.currentThread().getId() + "---" + System.currentTimeMillis()), 2, TimeUnit.SECONDS);
        //以固定的频率执行任务，开启任务的时间是固定的,在3s中之后执行，每隔2s执行一次任务
        scheduledExecutorService.scheduleAtFixedRate(() -> {
            System.out.println(Thread.currentThread().getId() + "---" + System.currentTimeMillis());
            try {
                //睡眠模拟任务执行时间，如果任务执行时间超过了时间间隔，立即执行下个任务
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, 3, 2, TimeUnit.SECONDS);

        //在上次任务结束后，在固定延迟后再次执行该任务，与任务执行时间无关，时间为：执行时间+延迟时间
        scheduledExecutorService.scheduleWithFixedDelay(
                () -> {
                    System.out.println(Thread.currentThread().getId() + "---" + System.currentTimeMillis());
                    try {
                        //睡眠模拟任务执行时间，如果任务执行时间超过了时间间隔，立即执行下个任务
                        TimeUnit.SECONDS.sleep(3);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }, 3, 2, TimeUnit.SECONDS
        );
```

#### 8.3 线程池的底层实现

##### 8.3.1 三大线程池

``` java
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }

```

核心线程数等于0，使用直接提交队列，意味着该线程池在极端情况下，每次提交新任务都会创建新的线程执行，适合用来执行大量耗时短且频繁提交的任务

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

核心线程数等于最大线程数，即创建一个指定大小的线程池

``` java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

核心线程数等于最大线程数等于1，在任意时刻只有1个线程在执行1个任务，多余的任务放在阻塞队列中

Excutors创建线程池的方法都使用了**ThreadPoolExecutor**线程池，这些方法都是对ThreadPoolExecutor的封装

```java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler) {
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.acc = System.getSecurityManager() == null ?
            null :
            AccessController.getContext();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```

- corePoolSize 指定线程池中核心线程的数量
- maximumPoolSize 指定线程池中最大线程数量
- keepAliveTime 当线程池中线程数量超过corePoolSize时，多余的空闲线程存活时间
- unit keepAliveTime时长单位
- workQueue 任务队列，提交未执行的任务队列，仅用于存储Runnable任务
- threadFactory 线程工厂，用于创建线程
- handler 拒绝策略



##### 8.3.2 队列

1. 直接提交队列
   - **SynchronousQueue**
   - 该队列没有容量，提交给线程池的任务不会被真实的保存，总是将新任务提交给线程执行，如果没有空闲线程，则尝试创建一个新的，如果线程池中的线程满了，则执行拒绝策略。
2. 有界任务队列
   - **ArrayBlockingQueue**
   - 创建该对象时可以指定一个容量，当有任务需要执行时，如果线程池中的线程数小于线程池的核心线程数corePoolSize，则创建新的线程；若线程池中线程大于线程池核心线程数corePoolSize则加入等待队列。如果队列已满则无法加入。线程池满时执行拒绝策略。
3. 无界任务队列
   - **LinkedBlockingQueue**
   - 与有界队列相比，除非系统资源耗尽，否则无界队列不存在任务入队失败的情况。当有新的任务时，在系统线程数小于corePoolSize核心线程则创建新的线程来执行任务；当线程池中线程数量corePoolSize大于核心线程数则将任务放入阻塞队列
4. 优先任务队列
   - **PriorityBlockingQueue**
   - 是特殊的无界队列，不管是**ArrayBlockingQueue**还是**LinkedBlockingQueue**都是按照先进先出的算法处理任务的





##### 8.3.3 拒绝策略

1. AbortPolicy （默认）
   - 会抛出异常
2. CallerRunsPolicy
   - 只要线程池没关闭，会在调用的线程中运行被丢弃的任务
3. DiscardOldestPolicy
   - 将任务队列中马上要执行的任务丢弃，尝试再次提交新任务
4. DiscardPolicy
   - 直接丢弃无法处理的任务



自定义拒绝策略

```java
Runnable runnable = () -> {
    int num = new Random().nextInt(5);
    System.out.println(Thread.currentThread().getId() + "---->" + System.currentTimeMillis() + "开始睡眠" + num + "s");
    try {
        TimeUnit.SECONDS.sleep(num);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
};
ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(5, 5, 0,
        TimeUnit.SECONDS, new LinkedBlockingQueue<>(10)
        , Executors.defaultThreadFactory(), (r, executor) -> System.out.println(r.toString() + " is discarding.."));


for (int i = 0; i < Integer.MAX_VALUE; i++) {
    threadPoolExecutor.submit(runnable);
}
```



##### 8.3.5 ThreadFactory

