## 什么叫线程安全？举例说明
多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的同步或者协同，这个类都能表现出正确的行为，那么就称这个类是线程安全的。
比如无状态对象一定是线程安全的。

##  进程和线程的区别
调度: 线程是调度的基本单位，进程是拥有资源的基本单位。同一进程的中线程的切换不会引起进程的切换，不同进程中进行线程切换会引起进程的切换。
拥有资源：进程是拥有资源的基本单位，线程除了自身的栈外一般不拥有资源。而是和其他线程共享同一进程中的资源。
系统开销：由于创建进程或者撤销进程时，系统都要分配和回收资源，如内存空间，I/O设备等，操作系统所付出的开销远大于创建或撤销进程时的开销。

## volatile的理解
**Volatile自身特性**：
Volatile 是轻量级的synchronized，它在多处理器开发过程中保证了共享变量的“可见性”，可见性是指当一个线程的某个共享变量发生改变时，另一个线程能够读取到这个修改的值。Voaltile变量修饰的变量在进行写操作时在多核处理器下首先将当前处理器缓存行的数据写回到系统内存中。为了保证一致性，其他处理器嗅探到总线上传播的数据，发现数据被修改了自己缓存地址的数据无效。
Volatile 可以禁止重排序，
Volatile 能保持单个简单volatile变量的读/写操作的具有原子性。但不能保证自增自减的原子性。

从**内存语义**来讲:
volatile变量的写-读与锁的释放-获取具有相同语义，volatile的写与锁的释放有相同的内存语义，volatile读与锁的获取具有相同语义。
线程A写一个volatile变量，实质上是线程A向接下来要读这个volatile变量的某个线程发出消息
线程B读一个volatile变量，实质上是线程B接收了之前某个线程发出的消息。
线程A写volatile变量，随后线程B读这个变量，这个过程实质上线程A通过内存向B发送消息。
内存语义的实现，也是禁止重排序特性：
为了实现volatile内存语义，JMM限制了对volatile重排序做了限制：
1.当第二个操作是volatile写时，不管第一个操作时什么，都不能重排序。
2.当第一个操作是volatile读时，不管第二个操作是什么，都不能重排序。
3.当第一个操作是volatile写，第二个操作是volatile读时，不重排序。
为了实现volatile的内存语义，编译器生成字节码时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。JMM采取保守策略。
1. 在每个volatile写操作前面插入一个StoreStore屏障
2. 在每个volatile写操作后面插入一个StoreLoad屏障
3. 在每个volatile读操作后面插入一个LoadLoad屏障
4. 在每个volatile读操作后面插入一个LoadStore屏障
		
## 原子性实现机制
处理器提供总线锁定和缓存锁定两种方式来保证复杂内存操作的原子性。
总线型：就是使用处理器提供一个LOCK信号，当一个处理器在总线传输信号时，其他处理器的请求将被阻塞住，那么该处理独占内存。所以总线锁定开销大。
缓存锁定：内存区域如果被缓存在缓存行中，且在在lock期间被锁定，当它执行锁操作写回内存时，处理器总线不在锁定而是通过修改内部的内存地址并使用缓存一致性制阻止同时修改保证操作的原子性。缓存一致性进制两个以上的处理器同时修改内存区域数据，其他处理器回写被锁定并且使其缓存行无效。

## Java原子性操作实现原理
使用循环CAS实现原子性操作，CAS是在操作期间先比较旧值，如果旧值没有发生改变，才交换成新值，发生了变化则不交换。这种方式会产生以下几种问题：1.ABA问题，通过加版本号解决；2.循环时间过长开销大，一般采用自旋方式实现；3. 只能保证一个共享变量的原子操作。 

## Java内存模型
Java内存模型控制线程之间的通信，决定了一个线程对共享变量的写入何时对另一个线程可见。它属于语言及的内存模型，它确保在不同编译器和不同的处理平台之上，通过禁止特定类型的编译器重排序和处理器重排序，为程序员提供一致的内存可见性保证。JMM的核心目标是找到一个好的平衡点，一方面是为程序员提供足够强的内存可见性保证（提供happens-before规则），另一方面对编译器和处理器的限制尽可能地放松（只要不改变程序结果，怎么优化都可以）

**可见性保证**
 为了提供内存可见性保证，JMM向程序员保证了以下hapens-before规则:
1.程序顺序规则：一个线程的每个操作happen-before与该线程的任意后续操作。
2.监视器锁规则：一个锁的解锁，happens-before于随后这个锁的加锁。
3.Volatile变量规则：对一个volatile域的写，happens-before于任意后续这个域的读。
4.传递性, 如果A happens-before B, 且B happens-before C 那么A happens-before C
5.线程启动规则：如果线程A执行操作ThreadB.start().那么线程A中的任意操作happens-before与线程B中的任意操作。
6.线程结束规则: 线程中的任何操作都必须在其线程检测到该线程已经结束之前执行，或者从Thread.join中成功返回，或者在调用Thread.isAlive时返回false.
7.中断规则:当一个线程在另一个线程上调用interrupt时，必须在被中断线程检测到interrupt调用之前执行(通过抛出InterruptedException,或者调用isInterrupted和interrupted)
8.终结器规则: 对象的构造函数必须在启动该对象的终结器之前执行完成。
 
**禁止重排序**
为了保证内存可见性，java编辑器在生成指令序列的适当位置插入内存屏障指令来禁止特定类型的处理器重排序。
重排序：编译器和处理器为了优化程序性能对指令进行重新排序的一种手段。
1)编译器优化的重排序: 编译器在不改变单线程程序语义的前提下可以重新安排语句顺序。
2)指令级并行的重排序.现代处理器采用指令级并行技术来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变对应指令的执行顺序。
3)内存系统重排序。由于处理器使用缓存和读/写缓冲区，这使得加载和存储操作看上去可能在乱序执行。

## Final域的内存语义
对于final域编译器和处理器要遵守两个重排序规则：
1.在构造器函数内对final域的写入，与随后把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。（保证了对象引用为任何线程可见之前，对象的final域已经被正确初始化过）
2.初次读一个包含final域的对象引用，与随后初次读这个final域这两个操作不能重排序。
为何保证其内存语义：可以为java程序员提供安全保证，只要对象是正确构造的，那么不需要使用同步就可以保证线程都能看到这个fianal域在构造函数中被初始化之后的值。

## 避免死锁的常见方法：
1)不免一个线程同时获取多个锁
2)避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源
3)尝试使用定时锁，使用lock.tryLock(timeout)来替代使用内部锁机制。
4)对数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况。

## 死锁的必要条件？怎么克服？
答：产生死锁的四个必要条件：
**互斥条件**：一个资源每次只能被一个进程使用。
**请求与保持条件**：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
**不剥夺条件**:进程已获得的资源，在末使用完之前，不能强行剥夺。
**循环等待条件**:若干进程之间形成一种头尾相接的循环等待资源关系。
这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

死锁的解决方法:
a 撤消陷于死锁的全部进程；
b 逐个撤消陷于死锁的进程，直到死锁不存在；
c 从陷于死锁的进程中逐个强迫放弃所占用的资源，直至死锁消失。
d 从另外一些进程那里强行剥夺足够数量的资源分配给死锁进程，以解除死锁状态

## CountDownLatch(闭锁) 与CyclicBarrier(栅栏)的区别
CountDownLatch: 允许一个或多个线程等待其他线程完成操作. CyclicBarrier：阻塞一组线程直到某个事件发生。 
1. 闭锁用于等待事件、栅栏是等待线程.
2. 闭锁CountDownLatch做减计数，而栅栏CyclicBarrier则是加计数。
3. CountDownLatch是一次性的，CyclicBarrier可以重用。
4. CountDownLatch一个线程(或者多个)，等待另外N个线程完成某个事情之后才能执行。CyclicBarrier是N个线程相互等待，任何一个线程完成之前，所有的线程都必须等待。 
CountDownLatch 是计数器, 线程完成一个就记一个,就像报数一样, 只不过是递减的.
而CyclicBarrier更像一个水闸, 线程执行就像水流, 在水闸处都会堵住, 等到水满(线程到齐)了, 才开始泄流.

## execute 和submit的区别
Execute()用于提交不需要返回值得任务，submit()用于提交需要返回值的任务，发挥Future类型的对象。

## Shutdown和shutdownNow的区别
它们的原理都是遍历线程池中的工作线程，然后逐个调用线程的Internet方法来中断线程，所以无法响应中断的任务可能永远无法终止。
ShutdownNow首先将线程池的状态设置成STOP,然后尝试停止所有正在执行或暂停的任务，并返回等待执行任务的列表。而shutdown只是将线程池设置成SHUTDOWN状态，然后中断没有正在执行任务的线程。

## ThreadLocal的设计理念与作用。
ThreadLocal并不是一个Thread，而是Thread的局部变量,当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本

http://blog.csdn.net/lufeng20/article/details/24314381
## 同步
同步就是协同步调，按预定的先后次序进行运行。

## sleep() 和 wait() 区别
答：sleep()方法是线程类（Thread）的静态方法，导致此线程暂停执行指定时间，将执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复（线程回到就绪（ready）状态），因为调用 sleep 不会释放对象锁。wait() 是 Object 类的方法，对此对象调用 wait()方法导致本线程放弃对象锁(线程暂停执行)，进入等待此对象的等待锁定池，只有针对此对象发出 notify 方法（或 notifyAll）后本线程才进入对象锁定池准备获得对象锁进入就绪状态。

## sleep() 和 yield() 区别
① sleep() 方法给其他线程运行机会时**不考虑线程的优先级**，因此会给低优先级的线程以运行的机会；yield() 方法**只会给相同优先级或更高优先级**的线程以运行的机会；
② 线程执行 sleep() 方法后转入**阻塞**（blocked）状态，而执行 yield() 方法后转入**就绪（ready）**状态；
③ sleep() 方法声明抛出InterruptedException，而 yield() 方法没有声明任何异常；
④ sleep() 方法比 yield() 方法（跟操作系统相关）具有更好的可移植性。

## 线程同步相关的方法。
**wait()**:使一个线程处于等待（阻塞）状态，并且释放所持有的对象的锁；
**sleep()**:使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要捕捉InterruptedException 异常；
**notify()**:唤醒一个处于等待状态的线程，当然在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且与优先级无关；
**notityAll()**:唤醒所有处入等待状态的线程，注意并不是给所有唤醒线程一个对象的锁，而是让它们竞争；

## 什么是线程池（thread pool）
答：在面向对象编程中，创建和销毁对象是很费时间的，因为创建一个对象要获取内存资源或者其它更多资源。在 Java 中更是如此，虚拟机将试图跟踪每一个对象，以便能够在对象销毁后进行垃圾回收。所以提高服务程序效率的一个手段就是尽可能减少创建和销毁对象的次数，特别是一些很耗资源的对象创建和销毁，这就是"池化资源"技术产生的原因。线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。

http://www.cnblogs.com/dolphin0520/p/3932921.html
## ConcurrentHashMap实现原理
ConcurrentHashMap和Hashtable主要区别就是围绕着锁的粒度以及如何锁。
Hashtabl在竞争激烈的环境下表现效率低下的原因是一把锁锁住整张表，导致所有线程同时竞争一个锁。ConcurrentHashMap采用分段锁，每把锁锁住容器中的一个Segment。那么多线程访问容器里不同的Segment的数据时线程就不会存在竞争，从而有效提高并发访问效率。首先是将数据分层多个Segment存储，并为每个Segment分配一把锁，当一个线程范围其中一段数据时，其他线程可以访问其他段的数据。

数据结构：
ConcurrentHashMap内部是有Segment数组和HashEntry数组组成。一个ConcurrentHashMapMap里包含一个Segment数组，而Segment的结构和HashMap一样，里面是由一个数组和链表结构组成，所以一个Segment内部包含一个HashEntry数组。每个HashEntry是一个链表结构，对于HashEntry数组进行修改时首先需要获取与它对应的Segment锁。默认情况下有16个Segment

Segment的定位:
使用Wang/Jenkins hash变种算法对元素的hashCode进行一次再散列，目的是为了减少散列冲突。

ConcurrentHashMap的操作:
1.) get
get操作实现非常简单高效。先经过一次在散列，然后用这个散列值通过散列运算定位到Segment，再通过散列算法定位到元素。get之所以高效是因为整个get过程不需要加锁，除非读到空值才会加锁重读。实现该技术的技术保证是保证HashEntry是不可变的。
第一步是访问count变量，这是一个volatile变量，由于所有的修改操作在进行结构修改时都会在最后一步写count 变量，通过这种机制保证get操作能够得到几乎最新的结构更新。对于非结构更新，也就是结点值的改变，由于HashEntry的value变量是 volatile的，也能保证读取到最新的值。接下来就是根据hash和key对hash链进行遍历找到要获取的结点，如果没有找到，直接访回null。
对hash链进行遍历不需要加锁的原因在于链指针next是final的。但是头指针却不是final的，这是通过getFirst(hash)方法返回，也就是存在 table数组中的值。这使得getFirst(hash)可能返回过时的头结点，例如，当执行get方法时，刚执行完getFirst(hash)之后，另一个线程执行了删除操作并更新头结点，这就导致get方法中返回的头结点不是最新的。这是可以允许，通过对count变量的协调机制，get能读取到几乎最新的数据，虽然可能不是最新的。要得到最新的数据，只有采用完全的同步。
2.) put
该方法也是在持有段锁(锁定当前segment)的情况下执行的，这当然是为了并发的安全，修改数据是不能并发进行的，必须得有个判断是否超限的语句以确保容量不足时能够rehash。首先根据计算得到的散列值定位到segment及该segment中的散列桶中。接着判断是否存在同样一个key的结点，如果存在就直接替换这个结点的值。否则创建一个新的结点并添加到hash链的头部，这时一定要修改modCount和count的值，同样修改count的值一定要放在最后一步。
3）remove
HashEntry中除了value不是final的，其它值都是final的，这意味着不能从hash链的中间或尾部添加或删除节点，因为这需要修改next 引用值，所有的节点的修改只能从头部开始。对于put操作，可以一律添加到Hash链的头部。但是对于remove操作，可能需要从中间删除一个节点，这就需要将要删除节点的前面所有节点整个复制一遍，最后一个节点指向要删除结点的下一个结点。
首先定位到要删除的节点e。如果不存在这个节点就直接返回null，否则就要将e前面的结点复制一遍，尾结点指向e的下一个结点。e后面的结点不需要复制，它们可以重用。
4) size()
每个Segment都有一个count变量，是一个volatile变量。当调用size方法时，首先先尝试2次通过不锁住segment的方式统计各个Segment的count值得总和，如果两次值不同则将锁住整个ConcurrentHashMap然后进行计算。
参见《java并发编程的艺术》P156
http://www.cnblogs.com/ITtangtang/p/3948786.html

## 线程的几种可用状态
线程在运行周期里有6中不同的状态。
1. New 新建
2. RUNNABLE 运行状态,操作系统中运行与就绪两种状态统称运行中
3. BLOCKED 阻塞状态
4. WAITING	等待状态
5. TIME_WAITING 超时等待
6. TERMINATED	终止状态

## 同步方法和同步代码块的区别是什么
1. 同步方法只能锁定当前对象或class对象， 而同步方法块可以使用其他对象、当前对象及当前对象的class作为锁。
2. 从反编译后的结果看，对于同步块使用了monitorenter和monitorexit指令，而同步方法则是依靠方法上的修饰符ACC_SYNCHRONIZED来完成，但它们的本质都是对一个对象监视器进行获取，而这个获取过程是排他的。
	
### 显示锁ReentrantLock与内置锁synchronized的相同与区别
相同：显示锁与内置锁在加锁和内存上提供的语义相同(互斥访问临界区)
不同：
1.使用方式，内置无需指定释放锁，简化锁操作。显示锁拥有锁获取和释放的可操作性。
2.功能上：显示锁提供了其他很多功能如定时锁等待、可中断锁等待、公平性、尝试非阻塞获取锁、以及实现非结构化的加锁。（一个线程获取不到锁而被阻塞在synchronized之外时，对该线程进行中断操作，此时该线程的中断表示为会被修改，但线程依旧会被阻塞在synchronized上，等待获取锁。）
3.对死锁的处理：内置只能重启，显示可以通过设置超时获取锁来避免
4.性能上：java1.5 显示远超内置，java1.6 显示锁稍微比内置好

7. atomicinteger和Volatile等线程安全操作的关键字的理解和使用

SOF你遇到过哪些情况。

21. 实现多线程的3种方法：Thread与Runable。
**1)继承Tread类，重写run函数**
**2)实现Runnable接口**
**3)实现Callable接口**


22. 线程同步的方法：sychronized、lock、reentrantLock等。

23. 锁的等级：方法锁、对象锁、类锁。
26. ThreadPool用法与优势。

26 Callable和Runnable的区别
27. Concurrent包里的其他东西：ArrayBlockingQueue、CountDownLatch等等。

29. foreach与正常for循环效率对比。
31. 反射的作用于原理。

32. 泛型常用特点，List<String>能否转为List<Object>。

36. 设计模式：单例、工厂、适配器、责任链、观察者等等。
37. JNI的使用。
java的代理是怎么实现的  
35. Java1.7与1.8新特性。
lmbda表达式
Java8新特性
连接池使用使用什么数据结构实现
实现连接池
结束一条 Thread 有什么方法？ interrupt 底层实现有看过吗？线程的状态是怎么样的？如果给你实现会怎么样做？
Java 中有内存泄露吗？是怎么样的情景？为什么不用循环计数？
java都有哪些加锁方式
AIO与BIO的区别
生产者与消费者，手写代码
1. Java创建线程之后，直接调用start()方法和run()的区别
2. 常用的线程池模式以及不同线程池的使用场景
3. newFixedThreadPool此种线程池如果线程数达到最大值后会怎么办，底层原理。
4. 多线程之间通信的同步问题，synchronized锁的是对象，衍伸出和synchronized相关很多的具体问题，例如同一个类不同方法都有synchronized锁，一个对象是否可以同时访问。或者一个类的static构造方法加上synchronized之后的锁的影响。
5. 了解可重入锁的含义，以及ReentrantLock 和synchronized的区别
6. 同步的数据结构，例如concurrentHashMap的源码理解以及内部实现原理，为什么他是同步的且效率高
8. 线程间通信，wait和notify

9. 定时线程的使用
10. 场景：在一个主线程中，要求有大量(很多很多)子线程执行完之后，主线程才执行完成。多种方式，考虑效率。
14. 并发、同步的接口或方法
16. J.U.C下的常见类的使用。 ThreadPool的深入考察； BlockingQueue的使用。（take，poll的区别，put，offer的区别）；原子类的实现。
17. 简单介绍下多线程的情况，从建立一个线程开始。然后怎么控制同步过程，多线程常用的方法和结构
19. 实现多线程有几种方式，多线程同步怎么做，说说几个线程里常用的方法


## 写出生产者消费者模式。

    public class ProducerConsumerPattern {
        public static void main(String args[]){
         BlockingQueue sharedQueue = new LinkedBlockingQueue();
         Thread prodThread = new Thread(new Producer(sharedQueue));
         Thread consThread = new Thread(new Consumer(sharedQueue));
         prodThread.start();
         consThread.start();
        }
    }
    
    //Producer Class in java
    class Producer implements Runnable {
        private final BlockingQueue sharedQueue;
        public Producer(BlockingQueue sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
        @Override
        public void run() {
            for(int i=0; i<10; i++){
                try {
                    System.out.println("Produced: " + i);
                    sharedQueue.put(i);
                } catch (InterruptedException ex) {
                    Logger.getLogger(Producer.class.getName()).log(Level.SEVERE, null, ex);
                }
            }
        }
    }
     
    //Consumer Class in Java
    class Consumer implements Runnable{
        private final BlockingQueue sharedQueue;
        public Consumer (BlockingQueue sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
        @Override
        public void run() {
            while(true){
                try {
                    System.out.println("Consumed: "+ sharedQueue.take());
                } catch (InterruptedException ex) {
                    Logger.getLogger(Consumer.class.getName()).log(Level.SEVERE, null, ex);
                }
            }
        }
    }


个人公众号(欢迎关注)：<br>
![](/assets/weix_gongzhonghao.jpg)





