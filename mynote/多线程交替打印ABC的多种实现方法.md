https://blog.csdn.net/xiaokang123456kao/article/details/77331878

# 多线程交替打印ABC的多种实现方法

![img](https://csdnimg.cn/release/phoenix/template/new_img/original.png)

[想作会飞的鱼](https://me.csdn.net/xiaokang123456kao) 2017-08-17 16:43:12 ![img](https://csdnimg.cn/release/phoenix/template/new_img/articleReadEyes.png) 37453 ![img](https://csdnimg.cn/release/phoenix/template/new_img/tobarCollect.png) 收藏 58

分类专栏： [Java多线程](https://blog.csdn.net/xiaokang123456kao/category_6944109.html) 文章标签： [多线程](https://www.csdn.net/gather_2d/MtTaEg0sMzM0MjItYmxvZwO0O0OO0O0O.html)[java](https://www.csdn.net/gather_24/NtTaIg5sMzYyLWJsb2cO0O0O.html)

版权

## 一、题目描述

建立三个线程A、B、C，A线程打印10次字母A，B线程打印10次字母B,C线程打印10次字母C，但是要求三个线程同时运行，并且实现交替打印，即按照ABCABCABC的顺序打印。

## 二、Synchronized同步法

### 1、基本思路

使用同步块和wait、notify的方法控制三个线程的执行次序。具体方法如下：从大的方向上来讲，该问题为三线程间的同步唤醒操作，主要的目的就是ThreadA->ThreadB->ThreadC->ThreadA循环执行三个线程。为了控制线程执行的顺序，那么就必须要确定唤醒、等待的顺序，所以每一个线程必须同时持有两个对象锁，才能进行打印操作。一个对象锁是prev，就是前一个线程所对应的对象锁，其主要作用是保证当前线程一定是在前一个线程操作完成后（即前一个线程释放了其对应的对象锁）才开始执行。还有一个锁就是自身对象锁。主要的思想就是，为了控制执行的顺序，必须要先持有prev锁（也就前一个线程要释放其自身对象锁），然后当前线程再申请自己对象锁，两者兼备时打印。之后首先调用self.notify()唤醒下一个等待线程（注意notify不会立即释放对象锁，只有等到同步块代码执行完毕后才会释放），再调用prev.wait()立即释放prev对象锁，当前线程进入休眠，等待其他线程的notify操作再次唤醒。

### 2、代码

经博友：璐璐的宝宝的指点，原程序虽然也能完成任务，但是存在一个很大的缺陷。为了对比一下，这里保留原实现，并给出新的改进实现。
**原实现**：

```
public class ABC_Synch {
    public static class ThreadPrinter implements Runnable {
        private String name;
        private Object prev;
        private Object self;

        private ThreadPrinter(String name, Object prev, Object self) {
            this.name = name;
            this.prev = prev;
            this.self = self;
        }

        @Override
        public void run() {
            int count = 10;
            while (count > 0) {// 多线程并发，不能用if，必须使用whil循环
                synchronized (prev) { // 先获取 prev 锁
                    synchronized (self) {// 再获取 self 锁
                        System.out.print(name);//打印
                        count--;

                        self.notifyAll();// 唤醒其他线程竞争self锁，注意此时self锁并未立即释放。
                    }
                    //此时执行完self的同步块，这时self锁才释放。
                    try {
                        prev.wait(); // 立即释放 prev锁，当前线程休眠，等待唤醒
                        /**
                         * JVM会在wait()对象锁的线程中随机选取一线程，赋予其对象锁，唤醒线程，继续执行。
                         */
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    public static void main(String[] args) throws Exception {
        Object a = new Object();
        Object b = new Object();
        Object c = new Object();
        ThreadPrinter pa = new ThreadPrinter("A", c, a);
        ThreadPrinter pb = new ThreadPrinter("B", a, b);
        ThreadPrinter pc = new ThreadPrinter("C", b, c);

        new Thread(pa).start();
        Thread.sleep(10);//保证初始ABC的启动顺序
        new Thread(pb).start();
        Thread.sleep(10);
        new Thread(pc).start();
        Thread.sleep(10);
    }
}1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253
```

运行结果：
![这里写图片描述](https://img-blog.csdn.net/20170817153052332)
可以看到程序一共定义了a,b,c三个对象锁，分别对应A、B、C三个线程。A线程最先运行，A线程按顺序申请c,a对象锁，打印操作后按顺序释放a,c对象锁，并且通过notify操作唤醒线程B。线程B首先等待获取A锁，再申请B锁，后打印B，再释放B，A锁，唤醒C。线程C等待B锁，再申请C锁，后打印C，再释放C,B锁，唤醒A。看起来似乎没什么问题，但如果你仔细想一下，就会发现有问题，就是初始条件，三个线程必须按照A,B,C的顺序来启动，但是这种假设依赖于JVM中线程调度、执行的顺序。

**原实现存在的问题**：
如果把上述代码放到eclipse上运行，可以发现程序虽然完成了交替打印ABC十次的任务，但是打印完毕后无法自动结束线程。这是为什么呢？原因就在于下面这段代码：

```
try {
            prev.wait(); // 立即释放 prev锁，当前线程休眠，等待唤醒
            /**
            * JVM会在wait()对象锁的线程中随机选取一线程，赋予其对象锁，唤醒线程，继续执行。
            */
            } catch (InterruptedException e) {
                e.printStackTrace();
            }12345678
```

prev.wait(); 是释放prev锁并休眠线程，等待唤醒。在最后一次打印完毕后，因为count为0，无法进入while循环的同步代码块，自然就不会触发notifyAll操作。这样一来，执行完打印操作后，线程就一直处于休眠待唤醒状态，导致线程无法正常结束。

**改进实现**：
我们找到了了问题的原因，解决起来就简单了。最直接的思路就是在最后一次打印操作时在不休眠线程的情况下释放对象锁，这可以通过notifyAll操作实现。于是改进的代码如下：

```
public class ABC_Synch {
    public static class ThreadPrinter implements Runnable {
        private String name;
        private Object prev;
        private Object self;

        private ThreadPrinter(String name, Object prev, Object self) {
            this.name = name;
            this.prev = prev;
            this.self = self;
        }

        @Override
        public void run() {
            int count = 10;
            while (count > 0) {// 多线程并发，不能用if，必须使用whil循环
                synchronized (prev) { // 先获取 prev 锁
                    synchronized (self) {// 再获取 self 锁
                        System.out.print(name);// 打印
                        count--;

                        self.notifyAll();// 唤醒其他线程竞争self锁，注意此时self锁并未立即释放。
                    }
                    // 此时执行完self的同步块，这时self锁才释放。
                    try {
                        if (count == 0) {// 如果count==0,表示这是最后一次打印操作，通过notifyAll操作释放对象锁。
                            prev.notifyAll();
                        } else {
                            prev.wait(); // 立即释放 prev锁，当前线程休眠，等待唤醒
                        }
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    public static void main(String[] args) throws Exception {
        Object a = new Object();
        Object b = new Object();
        Object c = new Object();
        ThreadPrinter pa = new ThreadPrinter("A", c, a);
        ThreadPrinter pb = new ThreadPrinter("B", a, b);
        ThreadPrinter pc = new ThreadPrinter("C", b, c);

        new Thread(pa).start();
        Thread.sleep(10);// 保证初始ABC的启动顺序
        new Thread(pb).start();
        Thread.sleep(10);
        new Thread(pc).start();
        Thread.sleep(10);
    }
}123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

上述代码放到eclipse上运行，就可以自动结束线程了。从这里，我们也可以得出wait和notify操作的异同：

- wait() 与 notify/notifyAll() 是Object类的方法，在执行两个方法时，要先获得锁。
- 当线程执行wait()时，会把当前的锁释放，然后让出CPU，进入等待状态。
- 当执行notify/notifyAll方法时，会唤醒一个处于等待该 对象锁 的线程，然后继续往下执行，直到执行完退出对象锁锁住的区域（synchronized修饰的代码块）后再释放锁。

从这里可以看出，notify/notifyAll()执行后，并不立即释放锁，而是要等到执行完临界区中代码后，再释放。所以在实际编程中，我们应该尽量在线程调用notify/notifyAll()后，立即退出临界区。即不要在notify/notifyAll()后面再写一些耗时的代码。

## 二、Lock锁方法

### 1、基本思路

通过ReentrantLock我们可以很方便的进行显式的锁操作，即获取锁和释放锁，对于同一个对象锁而言，统一时刻只可能有一个线程拿到了这个锁，此时其他线程通过lock.lock()来获取对象锁时都会被阻塞，直到这个线程通过lock.unlock()操作释放这个锁后，其他线程才能拿到这个锁。

### 2、代码

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ABC_Lock {
    private static Lock lock = new ReentrantLock();// 通过JDK5中的Lock锁来保证线程的访问的互斥
    private static int state = 0;//通过state的值来确定是否打印

    static class ThreadA extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10;) {
                try {
                    lock.lock();
                    while (state % 3 == 0) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("A");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10;) {
                try {
                    lock.lock();
                    while (state % 3 == 1) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("B");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10;) {
                try {
                    lock.lock();
                    while (state % 3 == 2) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("C");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    public static void main(String[] args) {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

运行结果
![这里写图片描述](https://img-blog.csdn.net/20170817155504338)

值得注意的是ReentrantLock是可重入锁，它持有一个锁计数器，当已持有锁的线程再次获得该锁时计数器值加1，每调用一次lock.unlock()时所计数器值减一，直到所计数器值为0，此时线程释放锁。示例如下：

```
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockTest {

    private ReentrantLock lock = new ReentrantLock();

    public void testReentrantLock() {
        // 线程获得锁
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " get lock");
            long beginTime = System.currentTimeMillis();
            while (System.currentTimeMillis() - beginTime < 100) {
            }
            //线程再次获得该锁（可重入）
            lock.lock();
            try {
                System.out.println(Thread.currentThread().getName() + " get lock again");
                long beginTime2 = System.currentTimeMillis();
                while (System.currentTimeMillis() - beginTime2 < 100) {
                }
            } finally {
                // 线程第一次释放锁
                lock.unlock();
                System.out.println(Thread.currentThread().getName() + " release lock");
            }
        } finally {
            // 线程再次释放锁
            lock.unlock();
            System.out.println(Thread.currentThread().getName() + " release lock again");
        }
    }

    public static void main(String[] args) {
        final ReentrantLockTest test = new ReentrantLockTest();
        Thread thread = new Thread(new Runnable() {
            public void run() {
                test.testReentrantLock();
            }
        }, "A");
        thread.start();
    }

}1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

![这里写图片描述](https://img-blog.csdn.net/20170817160325068)

## 三、ReentrantLock结合Condition

### 1、基本思路

与ReentrantLock搭配的通行方式是Condition，如下：

```
private Lock lock = new ReentrantLock();  
private Condition condition = lock.newCondition(); 
condition.await();//this.wait();  
condition.signal();//this.notify();  
condition.signalAll();//this.notifyAll();12345
```

Condition是被绑定到Lock上的，必须使用lock.newCondition()才能创建一个Condition。从上面的代码可以看出，Synchronized能实现的通信方式，Condition都可以实现，功能类似的代码写在同一行中。这样解题思路就和第一种方法基本一致，只是采用的方法不同。

### 2、代码

```
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ABC_Condition {
    private static Lock lock = new ReentrantLock();
    private static Condition A = lock.newCondition();
    private static Condition B = lock.newCondition();
    private static Condition C = lock.newCondition();

    private static int count = 0;

    static class ThreadA extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 0)//注意这里是不等于0，也就是说在count % 3为0之前，当前线程一直阻塞状态
                        A.await(); // A释放lock锁
                    System.out.print("A");
                    count++;
                    B.signal(); // A执行完唤醒B线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 1)
                        B.await();// B释放lock锁，当前面A线程执行后会通过B.signal()唤醒该线程
                    System.out.print("B");
                    count++;
                    C.signal();// B执行完唤醒C线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 2)
                        C.await();// C释放lock锁
                    System.out.print("C");
                    count++;
                    A.signal();// C执行完唤醒A线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778
```

![这里写图片描述](https://img-blog.csdn.net/20170817162420278)

## 四、Semaphore信号量方式

### 1、基本思路

Semaphore又称信号量，是操作系统中的一个概念，在Java并发编程中，信号量控制的是线程并发的数量。

```
public Semaphore(int permits)1
```

其中参数permits就是允许同时运行的线程数目;
Semaphore是用来保护一个或者多个共享资源的访问，Semaphore内部维护了一个计数器，其值为可以访问的共享资源的个数。一个线程要访问共享资源，先获得信号量，如果信号量的计数器值大于1，意味着有共享资源可以访问，则使其计数器值减去1，再访问共享资源。如果计数器值为0,线程进入休眠。当某个线程使用完共享资源后，释放信号量，并将信号量内部的计数器加1，之前进入休眠的线程将被唤醒并再次试图获得信号量。

Semaphore使用时需要先构建一个参数来指定共享资源的数量，Semaphore构造完成后即是获取Semaphore、共享资源使用完毕后释放Semaphore。

```
Semaphore semaphore = new Semaphore(10,true);  
semaphore.acquire();  
//do something here  
semaphore.release();  1234
```

### 2、代码

```
import java.util.concurrent.Semaphore;

public class ABC_Semaphore {
    // 以A开始的信号量,初始信号量数量为1
    private static Semaphore A = new Semaphore(1);
    // B、C信号量,A完成后开始,初始信号数量为0
    private static Semaphore B = new Semaphore(0);
    private static Semaphore C = new Semaphore(0);

    static class ThreadA extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    A.acquire();// A获取信号执行,A信号量减1,当A为0时将无法继续获得该信号量
                    System.out.print("A");
                    B.release();// B释放信号，B信号量加1（初始为0），此时可以获取B信号量
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    B.acquire();
                    System.out.print("B");
                    C.release();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    C.acquire();
                    System.out.println("C");
                    A.release();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

执行结果：
![这里写图片描述](https://img-blog.csdn.net/20170817163551932)

可以看到信号量的变化情况如下：
初始(A=1,B=0,C=0)—>第一次执行线程A时(A=1,B=0,C=0)—->第一次执行线程B时（A=0,B=1,C=0）—->第一次执行线程C时(A=0,B=0,C=1)—>第二次执行线程A(A=1,B=0,C=0)如此循环。