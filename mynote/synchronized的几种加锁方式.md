# synchronized的几种加锁方式

![img](https://csdnimg.cn/release/phoenix/template/new_img/original.png)

[宇学愈多](https://me.csdn.net/x1107761900) 2019-03-21 14:53:12 ![img](https://csdnimg.cn/release/phoenix/template/new_img/articleReadEyes.png) 4160 ![img](https://csdnimg.cn/release/phoenix/template/new_img/tobarCollect.png) 收藏 11

分类专栏： [java](https://blog.csdn.net/x1107761900/category_8777873.html)

版权

### 1、修饰普通方法（锁住的是当前实例对象）

- 同一个实例调用会阻塞
- 不同实例调用不会阻塞

```java
public class SynchronizedTest {
   //锁住了本类的实例对象
   public synchronized void test1() {
        try {
            logger.info(Thread.currentThread().getName() + " test1 进入了同步方法");
            Thread.sleep(5000);
            logger.info(Thread.currentThread().getName() + " test1 休眠结束");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        SynchronizedTest st = new SynchronizedTest();
        SynchronizedTest st2 = new SynchronizedTest();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test1();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st2.test1();
        }).start();

    }
}
```

此处列举的是不同实例调用的情况 

![img](https://img-blog.csdnimg.cn/20190321140437824.png)

### 2、同步代码块传参this（锁住的是当前实例对象）

- 同一个实例调用会阻塞
- 不同实例调用不会阻塞

```java
public class SynchronizedTest {
   //锁住了本类的实例对象
    public void test2() {
        synchronized (this) {
            try {
                logger.info(Thread.currentThread().getName() + " test2 进入了同步块");
                Thread.sleep(5000);
                logger.info(Thread.currentThread().getName() + " test2 休眠结束");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        SynchronizedTest st = new SynchronizedTest();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test2();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test2();
        }).start();

    }
}
```

此处列举的是同一实例调用的情况 

![img](https://img-blog.csdnimg.cn/20190321140918606.png)

### 3、同步代码块传参变量对象 （锁住的是变量对象）

- 同一个属性对象才会实现同步

```java
public class SynchronizedTest {
    
   public Integer lockObject;

    public SynchronizedTest(Integer lockObject) {
        this.lockObject = lockObject;
    }

   //锁住了实例中的成员变量
    public void test3() {
        synchronized (lockObject) {
            try {
                logger.info(Thread.currentThread().getName() + " test3 进入了同步块");
                Thread.sleep(5000);
                logger.info(Thread.currentThread().getName() + " test3 休眠结束");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public static void main(String[] args) {
        SynchronizedTest st = new SynchronizedTest(127);
        SynchronizedTest st2 = new SynchronizedTest(127);
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test3();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st2.test3();
        }).start();

    }
}
```

同一个实例对象的成员属性肯定是同一个，此处列举的是不同实例的情况，但是 依旧实现了同步，原因如下：

Integer存在静态缓存，范围是-128 ~ 127，当使用Integer A = 127 或者 Integer A = Integer.valueOf(127) 这样的形式，都是从此缓存拿。如果使用 Integer A = new Integer(127)，每次都是一个新的对象。此例中，两个对象实例的成员变量 lockObject 其实是同一个对象，因此实现了同步。还有字符串常量池也要注意。所以此处关注是，同步代码块传参的对象是否是同一个。这跟第二个方式其实是同一种。

![img](https://img-blog.csdnimg.cn/20190321141908149.png)

### 4、同步代码块传参class对象（全局锁）

-  所有调用该方法的线程都会实现同步

```java
public class SynchronizedTest {

   //全局锁，类是全局唯一的
    public void test4() {
        synchronized (SynchronizedTest.class) {
            try {
                logger.info(Thread.currentThread().getName() + " test4 进入了同步块");
                Thread.sleep(5000);
                logger.info(Thread.currentThread().getName() + " test4 休眠结束");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        SynchronizedTest st = new SynchronizedTest();
        SynchronizedTest st2 = new SynchronizedTest();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test4();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st2.test4();
        }).start();
    }
}
```

结果如下： 

 ![img](https://img-blog.csdnimg.cn/20190321145156385.png)

### 5、修饰静态方法（全局锁）

- 所有调用该方法的线程都会实现同步

```java
public class SynchronizedTest {

   //全局锁，静态方法全局唯一的
    public synchronized static void test5() {
        try {
            logger.info(Thread.currentThread().getName() + " test5 进入同步方法");
            Thread.sleep(5000);
            logger.info(Thread.currentThread().getName() + " test5 休眠结束");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    public static void main(String[] args) {
        SynchronizedTest st = new SynchronizedTest();
        SynchronizedTest st2 = new SynchronizedTest();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st.test5();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            st2.test5();
        }).start();
        new Thread(() -> {
            logger.info(Thread.currentThread().getName() + " test 准备进入");
            SynchronizedTest.test5();
        }).start();
    }
}
```

![img](https://img-blog.csdnimg.cn/20190321144604395.png)