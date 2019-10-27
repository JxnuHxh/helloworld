# JavaConcurrency
Java并发编程学习相关，主要跟着项目驱动学习并发编程

<b>1.线程跟进程？<b/><br>
线程是操作系统能够进行运算调度的最小单位，它是进程的子集，是进程中的实际运作单位<br>
一个进程可以有很多线程。每个进程都有自己的内存空间，是个实体，可执行代码和唯一进程标识符（PID）。 <br>
适当使用多线程可以提高运算效率，线程可以异步执行<br>
# 2.用户线程和守护线程的区别<br>
守护线程依赖于创建它的线程，而用户线程则不依赖。举个简单的例子：<br>
如果在main线程中创建了一个守护线程，当main方法运行完毕之后，<br>
守护线程也会随着消亡。而用户线程则不会，用户线程会一直运行直到其运行完毕。<br>
在JVM中，像垃圾收集器线程就是守护线程<br>
### 3.实现多线程的方法<br>
a.继承Thread类；<br>
b.实现Runnable接口；<br>
c.实现Callable接口通过FutureTask包装器来创建Thread线程；<br><br>
Thread也是Runna的实现类<br>
Runnable就相当于一个作业，而Thread才是真正的处理线程，我们需要的只是定义这个作业，<br><br>
然后将作业交给线程去处理，这样就达到了松耦合，也符合面向对象里面组合的使用，另外也<br><br>
节省了函数开销，继承Thread的同时，不仅拥有了作业的方法run()，还继承了其他所有的方法<br><br>
start()方法被用来启动新创建的线程，start()内部调用了run()方法<br>
### 4.线程的状态<br>
   new 状态是指线程刚创建, 尚未启动<br>
   Runnable  状态是线程正在正常运行中, 当然可能会有某种耗时计算/IO等待的操作/CPU时间<br>片切换等, 这个状态下发生的等待一般是其他系统资源, 而不是锁, Sleep等
   Blocked 这个状态下, 是在多个线程有同步操作的场景, 比如正在等待另一个线程的synchronized<br> 块的执行释放, 也就是这里是线程在等待进入临界区
  Waiting 这个状态下是指线程拥有了某个锁之后, 调用了他的wait方法, 等待其他线程/锁拥有者调用<br> notify / notifyAll 一遍该线程可以继续下一步操作, 这里要区分 BLOCKED 和 WATING 的区别, 一个是在临界点外面等待进入, 一个是在理解点里面wait等待别人notify, 线程调用了join方法 join了另外的线程的时候, 也会进入WAITING状态, 等待被他join的线程执行结束
   TIMED_WAITING 这个状态就是有限的(时间限制)的WAITING, 一般出现在调用wait(long), 
   join(long)<br>等情况下, 另外一个线程sleep后, 也会进入TIMED_WAITING状态
   TERMINATED 这个状态下表示 该线程的run方法已经执行完毕了, 基本上就等于死亡了
   (当时如果线程被<br>持久持有, 可能不会被回收)
   
###5.线程调度
wait()：使一个线程处于等待（阻塞）状态，并且释放所持有的对象的锁；
sleep()：使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要处理<br>
InterruptedException异常；<br>
notify()：唤醒一个处于等待状态的线程，当然在调用此方法的时候，并不能确切的唤醒<br>
某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且与优先级无关；<br>
notityAll()：唤醒所有处于等待状态的线程，该方法并不是将对象的锁给所有线程，<br>
而是让它们竞争，只有获得锁的线程才能进入就绪状态；<br>
sleep来自Thread类，和wait来自Object类<br>
###6.synchronized和Lock

synchronized关键字 表示对某个对象加锁 既有可见性 又有原子性<br>
读写都应该加锁  不然可能会出现脏读  但是会影响性能<br>
一个同步方法可以调用另一个同步方法,一个线程已经拥有某个对象的锁<br>
再次申请的时候仍然会得到该对象的锁,也就是说synchronized获得的锁是<br>
可重入的(线程可以进入任何一个它已经拥有的锁所同步着的代码块)<br>
主要相同点：Lock能完成Synchronized所实现的所有功能。
主要不同点：Lock有比Synchronized更精确的线程予以和更好的性能。<br>
Synchronized会自动释放锁，但是Lock一定要求程序员手工释放，并且必须在finally从句中释放。

###7.什么是线程安全？举例说明
如果每次运行结果和单线程运行结果一样，而且其他变量结果的值和预期效果也是一样的
就是线程安全的<br>
ArrayList和Vector有什么区别？HashMap和HashTable有什么区别？StringBuilder和<br>
StringBuffer有什么区别？这些都是Java面试中常见的基础问题。面对这样的问题，回答是：<br>
ArrayList是非线程安全的，Vector是线程安全的；HashMap是非线程安全的，HashTable<br>
是线程安全的；StringBuilder是非线程安全的，StringBuffer是线程安全的<br>


   
   
   程序在执行的过程中，如果出现异常，默认情况锁会被释放
   所以，在并发处理的过程中，有异常要多加小心，不然可能会发生不一致的情况
   比如在一个web app处理过程中，多个servlet线程共同访问同一个资源，这时异常情况处理不合适，
   在一个线程中抛出异常，其他线程就会进入同步代码区，有可能会访问到异常产生时数据。
   因此要非常小心的处理同步业务逻辑中的异常
   
   volatile 关键字，使一个变量在多个线程间可见
          
          A B线程都用到一个变量，java默认是A线程中保留一份copy，这样如果B线程修改了该变量，则A线程未必知道
          
            使用volatile关键字，会让所有线程都会读到变量的修改值
          
           
           在下面的代码中，running是存在于堆内存的t对象中
           当线程t1开始运行的时候，会把running值从内存中读到t1线程的工作区，在运行过程中直接使用这个copy，并不会每次都去
            读取堆内存，这样，当主线程修改running的值之后，t1线程感知不到，所以不会停止运行
           
            使用volatile，将会强制所有线程都去堆内存中读取running的值
           
            可以阅读这篇文章进行更深入的理解
            http://www.cnblogs.com/nexiyi/p/java_memory_model_and_thread.html
           
           volatile并不能保证多个线程共同修改running变量时所带来的不一致问题，也就是说volatile不能替代synchronized