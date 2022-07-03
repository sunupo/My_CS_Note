# [1.ExecutorService 的理解和使用](https://www.cnblogs.com/jfaith/p/11114470.html)

## new Thread问题

前言：

我们之前使用线程的时候都是使用new Thread来进行线程的创建，但是这样会有一些问题。如：

a. 每次new Thread新建对象性能差。
b. 线程缺乏统一管理，可能无限制新建线程，相互之间竞争，及可能占用过多系统资源导致死机或oom。
c. 缺乏更多功能，如定时执行、定期执行、线程中断。
相比new Thread，Java提供的四种线程池的好处在于：
a. 重用存在的线程，减少对象创建、消亡的开销，性能佳。
b. 可有效控制最大并发线程数，提高系统资源的使用率，同时避免过多资源竞争，避免堵塞。
c. 提供定时执行、定期执行、单线程、并发数控制等功能。

而我们今天来学习和掌握另外一个新的技能，特别像一个线程池的一个接口类ExecutorService，下面我们来了解下java中Executors的线程池

Java通过Executors提供四种线程池，分别为：
newCachedThreadPool创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
newFixedThreadPool 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

### (1). newCachedThreadPool

创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。示例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
for (int i = 0; i < 10; i++) {
    final int index = i;
    try {
        Thread.sleep(index * 1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    cachedThreadPool.execute(new Runnable() {

        @Override
        public void run() {
            System.out.println(index);
        }
    });
}
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。


### (2). newFixedThreadPool

创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。示例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
for (int i = 0; i < 10; i++) {
    final int index = i;
    fixedThreadPool.execute(new Runnable() {


        @Override
        public void run() {
            try {
                System.out.println(index);
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    });
}
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

因为线程池大小为3，每个任务输出index后sleep 2秒，所以每两秒打印3个数字。
定长线程池的大小最好根据系统资源进行设置。如Runtime.getRuntime().availableProcessors()。可参考PreloadDataCache。


### (3) newScheduledThreadPool

创建一个定长线程池，支持定时及周期性任务执行。延迟执行示例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
scheduledThreadPool.schedule(new Runnable() {

    @Override
    public void run() {
        System.out.println("delay 3 seconds");
    }
}, 3, TimeUnit.SECONDS);
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

表示延迟3秒执行。

定期执行示例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

scheduledThreadPool.scheduleAtFixedRate(new Runnable() {

@Override
public void run() {
System.out.println("delay 1 seconds, and excute every 3 seconds");
}
}, 1, 3, TimeUnit.SECONDS);

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

表示延迟1秒后每3秒执行一次。
ScheduledExecutorService比Timer更安全，功能更强大，后面会有一篇单独进行对比。


### (4)newSingleThreadExecutor

创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。示例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
for (int i = 0; i < 10; i++) {
    final int index = i;
    singleThreadExecutor.execute(new Runnable() {

        @Override
        public void run() {
            try {
                System.out.println(index);
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    });
}博客来源：https://www.cnblogs.com/zhaoyan001/p/7049627.html
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

分类: [线程池](https://www.cnblogs.com/jfaith/category/1493882.html)

# 2.FutureTask与Future

## 1.FutureTask+Callable(不使用线程池执行)

attention：ft最多等7000ms，超时过后会有一个TimeoutException 异常；因为此处设置的5000ms，所在在5000ms的时候ft.get(7000,TimeUnit.MILLISECONDS)就会有返回结果。

~~~java
FutureTask<Integer> ft = new FutureTask<Integer>(new Callable<Integer>() {
	@Override
	public Integer call() throws Exception {
		Thread.sleep(5000);
		return 333;
	}
});
Thread t1 = new Thread(ft);
t1.start();
try {
	System.out.println(ft.get(7000,TimeUnit.MILLISECONDS));
} catch (InterruptedException e3) {
	e3.printStackTrace();
} catch (ExecutionException e3) {
	e3.printStackTrace();
} catch (TimeoutException e) {
	e.printStackTrace();
}

~~~

## 2.结合线程池执行

### 1.FutureTask

~~~java
ExecutorService executorService = Executors.newCachedThreadPool();
FutureTask  task = new  FutureTask(new Callable<Integer>() {
	@Override
	public Integer call() throws Exception {
		return 123;
	}
});
executorService.submit(task);
try {
	System.out.println(task.get());
    //System.out.println(submit.get(3000,TimeUnit.MILLISECONDS)); // 设置超时

} catch (InterruptedException e2) {
	e2.printStackTrace();
} catch (ExecutionException e2) {
	e2.printStackTrace();
} catch (TimeoutException e) {
	e.printStackTrace();
}

~~~

**注意**：把FutureTask通过Excutor提交过后（executorService.submit(task)）返回的Future对象来get（submit.get），是获取不到结果的。

本来是用FutureTask来返回结果的，如果接着把FutureTask提交到线程池，获取的返回结果为Future，executorService.submit(task)；

~~~java
ExecutorService executorService = Executors.newCachedThreadPool();
FutureTask  task = new  FutureTask(new Callable<Integer>() {
	@Override
	public Integer call() throws Exception {
		return 123;
	}
});
Future<?> submit = executorService.submit(task);  // 尝试用Future的对象submit来get结果
try {
	System.out.println(submit.get(3000,TimeUnit.MILLISECONDS));  // submit 是get不到结果的。
	System.out.println(task.get());
} catch (InterruptedException e2) {
	e2.printStackTrace();
} catch (ExecutionException e2) {
	e2.printStackTrace();
} catch (TimeoutException e) {
	e.printStackTrace();
}
~~~

### 2.FutureTask

~~~java
ExecutorService executorService = Executors.newCachedThreadPool();
Future<Integer> futureTask = executorService.submit(
		new Callable<Integer>() {
	@Override
	public Integer call() throws Exception {
		Thread.sleep(1000);
		return 456;
	}
});
try {
	System.out.println(futureTask.get(3000,TimeUnit.MILLISECONDS));
} catch (InterruptedException e1) {
	// TODO Auto-generated catch block
	e1.printStackTrace();
} catch (ExecutionException e1) {
	// TODO Auto-generated catch block
	e1.printStackTrace();
} catch (TimeoutException e1) {
	// TODO Auto-generated catch block
	e1.printStackTrace();
	futureTask.cancel(true);	
}
~~~

# Java线程池七个参数详解

## 线程池中的BlockingQueue

首先看下构造函数

```
public ThreadPoolExecutor(int corePoolSize,
                         int maximumPoolSize,
                         long keepAliveTime,
                         TimeUnit unit,
                         BlockingQueue<Runnable> workQueue,
                         ThreadFactory threadFactory,
                         RejectedExecutionHandler handler){...}
```

TimeUnit:时间单位；BlockingQueue：等待的线程存放队列；keepAliveTime：非核心线程的闲置超时时间，超过这个时间就会被回收；RejectedExecutionHandler:线程池对拒绝任务的处理策略。
自定义线程池：这个构造方法对于队列是什么类型比较关键。

- 在使用有界队列时，若有新的任务需要执行，如果线程池实际线程数小于corePoolSize，则优先创建线程，
- 若大于corePoolSize，则会将任务加入队列，
- 若队列已满，则在总线程数不大于maximumPoolSize的前提下，创建新的线程，
- 若队列已经满了且线程数大于maximumPoolSize，则执行拒绝策略。或其他自定义方式。

个人独立博客：https://blog.it-follower.com/posts/1035400434.html

java多线程开发时，常常用到线程池技术，这篇文章是对创建java线程池时的七个参数的详细解释。

![img](https://img-blog.csdnimg.cn/20190423104753143.png)

从源码中可以看出，线程池的构造函数有7个参数，分别是corePoolSize、maximumPoolSize、keepAliveTime、unit、workQueue、threadFactory、handler。下面会对这7个参数一一解释。

## 一、corePoolSize 线程池核心线程大小

线程池中会维护一个最小的线程数量，即使这些线程处理空闲状态，他们也不会 被销毁，除非设置了allowCoreThreadTimeOut。这里的最小线程数量即是corePoolSize。

## 二、maximumPoolSize 线程池最大线程数量

一个任务被提交到线程池后，首先会缓存到工作队列（后面会介绍）中，如果工作队列满了，则会创建一个新线程，然后从工作队列中的取出一个任务交由新线程来处理，而将刚提交的任务放入工作队列。线程池不会无限制的去创建新线程，它会有一个最大线程数量的限制，这个数量即由maximunPoolSize来指定。

## 三、keepAliveTime 空闲线程存活时间

一个线程如果处于空闲状态，并且当前的线程数量大于corePoolSize，那么在指定时间后，这个空闲线程会被销毁，这里的指定时间由keepAliveTime来设定

## 四、unit 空间线程存活时间单位

keepAliveTime的计量单位

## 五、workQueue 工作队列

新任务被提交后，会先进入到此工作队列中，任务调度时再从队列中取出任务。jdk中提供了四种工作队列：

### ①ArrayBlockingQueue

基于数组的有界阻塞队列，按FIFO排序。新任务进来后，会放到该队列的队尾，有界的数组可以防止资源耗尽问题。当线程池中线程数量达到corePoolSize后，再有新任务进来，则会将任务放入该队列的队尾，等待被调度。如果队列已经是满的，则创建一个新线程，如果线程数量已经达到maxPoolSize，则会执行拒绝策略。

### ②LinkedBlockingQuene

基于链表的无界阻塞队列（其实最大容量为Interger.MAX），按照FIFO排序。由于该队列的近似无界性，当线程池中线程数量达到corePoolSize后，再有新任务进来，会一直存入该队列，而不会去创建新线程直到maxPoolSize，因此使用该工作队列时，参数maxPoolSize其实是不起作用的。

### ③SynchronousQuene

一个不缓存任务的阻塞队列，生产者放入一个任务必须等到消费者取出这个任务。也就是说新任务进来时，不会缓存，而是直接被调度执行该任务，如果没有可用线程，则创建新线程，如果线程数量达到maxPoolSize，则执行拒绝策略。

### ④PriorityBlockingQueue

具有优先级的无界阻塞队列，优先级通过参数Comparator实现。

## 六、threadFactory 线程工厂

创建一个新线程时使用的工厂，可以用来设定线程名、是否为daemon线程等等

## 七、handler 拒绝策略

当工作队列中的任务已到达最大限制，并且线程池中的线程数量也达到最大限制，这时如果有新任务提交进来，该如何处理呢。这里的拒绝策略，就是解决这个问题的，jdk中提供了4中拒绝策略：

### ①CallerRunsPolicy

该策略下，在调用者线程中直接执行被拒绝任务的run方法，除非线程池已经shutdown，则直接抛弃任务。

![img](https://img-blog.csdnimg.cn/20190423110958685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llMTcxODY=,size_16,color_FFFFFF,t_70)

### ②AbortPolicy

该策略下，直接丢弃任务，并抛出RejectedExecutionException异常。

![img](https://img-blog.csdnimg.cn/20190423111144317.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llMTcxODY=,size_16,color_FFFFFF,t_70)

### ③DiscardPolicy

该策略下，直接丢弃任务，什么都不做。

![img](https://img-blog.csdnimg.cn/20190423111218910.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llMTcxODY=,size_16,color_FFFFFF,t_70)

### ④DiscardOldestPolicy

该策略下，抛弃进入队列最早的那个任务，然后尝试把这次拒绝的任务放入队列

![img](https://img-blog.csdnimg.cn/20190423111311406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llMTcxODY=,size_16,color_FFFFFF,t_70)

 

到此，构造线程池时的七个参数，就全部介绍完毕了。