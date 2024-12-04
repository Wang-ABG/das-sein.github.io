
## 1. 概述

在现代并发编程中，任务的顺序控制是一个常见且重要的挑战，尤其是当多个线程需要协同工作并等待某些任务完成后再继续执行时。Java 提供了多种并发工具来帮助开发者解决这些问题，其中 **`CountDownLatch`** 就是一个非常有效的工具。`CountDownLatch` 允许一个或多个线程等待其他线程完成某些操作后再继续执行。

> [!NOTE] 
>**`CountDownLatch`** 是一种计数器同步工具，适用于那些需要等待其他线程完成任务后再继续执行的场景。

## 2. CountDownLatch 的工作原理

### 2.1 `CountDownLatch` 的基本概念

`CountDownLatch` 是一个同步工具类，提供了一个计数器，计数器的初始值可以在构造时设置。每当一个线程调用 `countDown()` 方法时，计数器减一。当计数器减到零时，所有调用了 `await()` 方法的线程将被唤醒，继续执行后续的任务。

> [!TIP] 
>`CountDownLatch` 在初始化时设置一个计数器，随着线程的执行，计数器递减。计数器归零时，所有等待线程才被唤醒。

### 2.2 计数器递减的机制

- **初始化时设置计数器的值**：在 `CountDownLatch` 创建时，我们传入一个初始计数值（例如 `4`）。这个计数器用于表示需要等待的任务数量。
- **每次调用 `countDown()` 时递减计数器**：当每个线程完成任务后，调用 `countDown()` 方法，计数器值递减。
- **等待计数器归零**：主线程调用 `await()` 方法后，会被阻塞，直到计数器的值减到零时才会被唤醒，继续执行。

> [!IMPORTANT] 
>`CountDownLatch` 的关键点在于它允许多个线程等待直到计数器归零后再继续执行，这对于需要等待多个并发任务完成后聚合结果的场景非常有效。

### 2.3 适用场景

`CountDownLatch` 最常用于那些要求多个任务并行执行且需要等待所有任务完成才能继续后续操作的场景。例如，在某些场景下，多个线程同时进行计算任务，计算结果最终需要由主线程统一汇总处理。

> [!CAUTION] 
>`CountDownLatch` 不支持重置操作。如果计数器已经为零，就无法重新设置。若需要重置功能，可以考虑使用 **`CyclicBarrier`** 或 **`Semaphore`**。

------

## 3. 核心源码解析

### 3.1 `CountDownLatch` 的构造方法

`CountDownLatch` 的构造方法接受一个整数参数，表示初始计数器的值。内部的计数器使用 AQS（AbstractQueuedSynchronizer）来实现：

```java
public CountDownLatch(int count) {
    if (count < 0) throw new IllegalArgumentException("count < 0");
    // 直接使用 AQS 的 state 来存储计数
    this.sync = new Sync(count);
}

private static final class Sync extends AbstractQueuedSynchronizer {
    Sync(int count) {
        // 设置初始状态
        setState(count);
    }
}
```

这里，`Sync` 是一个继承自 `AbstractQueuedSynchronizer` 的内部类，它直接使用 AQS 的状态 `state` 来保存计数器的值。当计数器为零时，AQS 会允许线程继续执行。

### 3.2 `await()` 方法的实现

`await()` 方法是主线程等待计数器归零的关键方法。它通过调用 `acquireSharedInterruptibly(1)` 来获取共享锁。当计数器不为零时，当前线程会被阻塞，直到计数器归零时，线程才会被唤醒。

```java
public void await() throws InterruptedException {
    sync.acquireSharedInterruptibly(1);
}

public boolean await(long timeout, TimeUnit unit) 
        throws InterruptedException {
    return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
}

public final void acquireSharedInterruptibly(int arg)
        throws InterruptedException {
    if (Thread.interrupted())
        throw new InterruptedException();
    if (tryAcquireShared(arg) < 0)
        doAcquireSharedInterruptibly(arg);
}

/*  AQS子类实现   */
protected int tryAcquireShared(int acquires) {
    // 当 state 为 0 时，允许通过
    return getState() == 0 ? 1 : -1;
}



private void doAcquireSharedInterruptibly(int arg) throws InterruptedException {
    // 添加节点至等待队列
    final Node node = addWaiter(Node.SHARED);
    boolean failed = true;
    try {
        for (;;) { // 无限循环
            // 获取node的前驱节点
            final Node p = node.predecessor();
            if (p == head) { // 前驱节点为头节点
                // 试图在共享模式下获取对象状态
                int r = tryAcquireShared(arg);
                if (r >= 0) { // 获取成功
                    // 设置头节点并进行繁殖
                    setHeadAndPropagate(node, r);
                    // 设置节点next域
                    p.next = null; // help GC
                    failed = false;
                    return;
                }
            }
            if (shouldParkAfterFailedAcquire(p, node) &&
                parkAndCheckInterrupt()) // 在获取失败后是否需要禁止线程并且进行中断检查
                // 抛出异常
                throw new InterruptedException();
        }
    } finally {
        if (failed)
            cancelAcquire(node);
    }
}

private void setHeadAndPropagate(Node node, int propagate) {
    // 获取头节点
    Node h = head; // Record old head for check below
    // 设置头节点
  
    // 进行判断
    if (propagate > 0 || h == null || h.waitStatus < 0 ||
        (h = head) == null || h.waitStatus < 0) {
        // 获取节点的后继
        Node s = node.next;
        if (s == null || s.isShared()) // 后继为空或者为共享模式
            // 以共享模式进行释放
            doReleaseShared();
    }
}
```

> [!IMPORTANT] 
>`await()` 方法的调用会使当前线程被阻塞，直到 `CountDownLatch` 内部的计数器递减到零，所有等待的线程才会被唤醒。

### 3.3 `countDown()` 方法的实现

`countDown()` 方法用于递减计数器，直到计数器为零时，所有在 `await()` 方法中等待的线程才会被唤醒。该方法通过 `releaseShared()` 调用内部的 `tryReleaseShared()` 来更新计数器的值。

```java
public void countDown() {
    sync.releaseShared(1);
}

public final boolean releaseShared(int arg) {
    if (tryReleaseShared(arg)) {
        doReleaseShared();
        return true;
    }
    return false;
}

/* 由 AQS 的子类实现*/
protected boolean tryReleaseShared(int releases) {
    // 无限循环
    for (;;) {
        // 获取状态
        int c = getState();
        if (c == 0) // 没有被线程占有
            return false;
        // 下一个状态
        int nextc = c-1;
        if (compareAndSetState(c, nextc)) // 比较并且设置成功
            return nextc == 0;
    }
}


private void doReleaseShared() {
    // 无限循环
    for (;;) {
        // 保存头节点
        Node h = head;
        if (h != null && h != tail) { // 头节点不为空并且头节点不为尾结点
            // 获取头节点的等待状态
            int ws = h.waitStatus; 
            if (ws == Node.SIGNAL) { // 状态为SIGNAL
                if (!compareAndSetWaitStatus(h, Node.SIGNAL, 0)) // 不成功就继续
                    continue;            // loop to recheck cases
                // 释放后继结点
                unparkSuccessor(h);
            }
            else if (ws == 0 &&
                        !compareAndSetWaitStatus(h, 0, Node.PROPAGATE)) // 状态为0并且不成功，继续
                continue;                // loop on failed CAS
        }
        if (h == head) // 若头节点改变，继续循环  
            break;
    }
}

```

> [!WARNING]
> `countDown()` 方法递减计数器时，如果计数器已经为零，再次调用 `countDown()` 不会有任何效果。

### 3.4 AQS 的作用

`CountDownLatch` 是基于 AQS 共享模式实现的，而 AQS 是一个提供底层同步机制的抽象类。AQS 提供了一套用于同步操作的方法，如 `acquireShared` 和 `releaseShared`，以及 `tryAcquireShared` 和 `tryReleaseShared` 等用于共享模式的接口。

AQS 会在计数器递减至零时，自动释放在 `await()` 方法中等待的线程。通过 CAS 操作保证了计数器操作的线程安全性。

> [!TIP] 
> AQS 提供了一种通用的并发控制机制，简化了同步器的开发。在 `CountDownLatch` 中，AQS 通过 `state` 字段控制计数器的递减和线程的等待。

------

## 4. 使用示例：模拟统计报表

### 4.1 功能现状

运营系统中，存在一个统计报表页面，展示多项指标，如每日新增用户数量、订单数量、商品销量、总销售额等。这些数据量较大，且涉及多个业务领域，因此页面加载速度较慢，尤其是统计报表的页面，加载时间往往接近一分钟。

### 4.2 问题分析

每个统计指标数据的获取需要单独查询数据库，单个指标的统计时间大约为 3 秒。由于页面需要展示 10 个以上的统计指标，因此如果采用串行统计的方式，整体统计时间可能达到近一分钟（即 `3秒 × 10个指标`）。为了提高页面渲染效率，必须将统计过程从串行化改为并行化，从而缩短整体的统计时间。

### 4.3 解决方案

考虑到每个统计任务独立，且统计过程是独立的，最合适的优化方法就是采用多线程并行执行。每个统计任务可以由一个单独的线程执行，这样每个统计指标就能并行计算，从而大幅减少页面加载时间。

#### 关键点：

- 使用 **`CountDownLatch`** 来实现主线程等待所有子线程执行完毕再继续执行。
- 子线程在执行完各自的统计任务后调用 `countDown()`，主线程则通过 `await()` 等待计数器归零后聚合结果。

### 4.4 解决方案步骤

1. 创建并发线程，每个线程负责统计一个指标。
2. 主线程等待所有子线程完成后，聚合结果并返回给前端渲染。
3. 使用 **`CountDownLatch`** 确保所有子线程完成任务后，主线程才继续执行。

> [!CAUTION] 
> 当并行执行任务时，确保任务的执行顺序不影响最终结果，`CountDownLatch` 可以帮助我们保证所有任务都完成后再执行后续逻辑。



### 4.5 代码实现

```java
//用于聚合所有的统计指标
private static Map map=new HashMap();
//创建计数器，这里需要统计4个指标
private static CountDownLatch countDownLatch=new CountDownLatch(4);

public static void main(String[] args) {

    //记录开始时间
    long startTime=System.currentTimeMillis();

    Thread countUserThread=new Thread(new Runnable() {
        public void run() {
            try {
                System.out.println("正在统计新增用户数量");
                Thread.sleep(3000);//任务执行需要3秒
                map.put("userNumber",1);//保存结果值
                countDownLatch.countDown();//标记已经完成一个任务
                System.out.println("统计新增用户数量完毕");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    });
    Thread countOrderThread=new Thread(new Runnable() {
        public void run() {
            try {
                System.out.println("正在统计订单数量");
                Thread.sleep(3000);//任务执行需要3秒
                map.put("countOrder",2);//保存结果值
                countDownLatch.countDown();//标记已经完成一个任务
                System.out.println("统计订单数量完毕");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    });

    Thread countGoodsThread=new Thread(new Runnable() {
        public void run() {
            try {
                System.out.println("正在商品销量");
                Thread.sleep(3000);//任务执行需要3秒
                map.put("countGoods",3);//保存结果值
                countDownLatch.countDown();//标记已经完成一个任务
                System.out.println("统计商品销量完毕");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    });

    Thread countmoneyThread=new Thread(new Runnable() {
        public void run() {
            try {
                System.out.println("正在总销售额");
                Thread.sleep(3000);//任务执行需要3秒
                map.put("countmoney",4);//保存结果值
                countDownLatch.countDown();//标记已经完成一个任务
                System.out.println("统计销售额完毕");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    });
    //启动子线程执行任务
    countUserThread.start();
    countGoodsThread.start();
    countOrderThread.start();
    countmoneyThread.start();

    try {
        //主线程等待所有统计指标执行完毕
        countDownLatch.await();
        long endTime=System.currentTimeMillis();//记录结束时间
        System.out.println("------统计指标全部完成--------");
        System.out.println("统计结果为："+map.toString());
        System.out.println("任务总执行时间为"+(endTime-startTime)/1000+"秒");

    } catch (InterruptedException e) {
        e.printStackTrace();
    }
```



```java
import java.util.concurrent.*;
import java.util.Map;
import java.util.HashMap;

public class ThreadPoolExample {
    // 用于聚合所有的统计指标
    private static Map<String, Integer> map = new ConcurrentHashMap<>();
    // 创建计数器，这里需要统计 4 个指标
    private static CountDownLatch countDownLatch = new CountDownLatch(4);

    public static void main(String[] args) {
        // 记录开始时间
        long startTime = System.currentTimeMillis();

        // 创建线程池
        ExecutorService executorService = Executors.newFixedThreadPool(4);

        // 提交任务到线程池
        executorService.submit(() -> countTask("新增用户数量", "userNumber", 1, 3000));
        executorService.submit(() -> countTask("订单数量", "countOrder", 2, 3000));
        executorService.submit(() -> countTask("商品销量", "countGoods", 3, 3000));
        executorService.submit(() -> countTask("总销售额", "countMoney", 4, 3000));

        try {
            // 主线程等待所有统计指标执行完毕
            countDownLatch.await();
            long endTime = System.currentTimeMillis();
            System.out.println("------统计指标全部完成--------");
            System.out.println("统计结果为：" + map.toString());
            System.out.println("任务总执行时间为 " + (endTime - startTime) / 1000 + " 秒");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            executorService.shutdown(); 
        } 
    }
    
    private static void countTask(String taskName, String key, int value, int delay) {
    try {
        System.out.println("正在统计 " + taskName);
        Thread.sleep(delay); // 模拟任务执行的时间
        map.put(key, value); // 保存结果值
        System.out.println("统计 " + taskName + " 完毕");
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        countDownLatch.countDown(); // 标记任务完成
    }
}
}
```



### 4.6 输出结果

```plaintext
正在统计 新增用户数量
正在统计 订单数量
正在统计 商品销量
正在统计 总销售额
统计 新增用户数量 完毕
统计 订单数量 完毕
统计 商品销量 完毕
统计 总销售额 完毕
------统计指标全部完成--------
统计结果为：{userNumber=1, countOrder=2, countGoods=3, countMoney=4}
任务总执行时间为 3 秒
```

> [!TIP] 
> 通过并行执行统计任务，整体执行时间被显著缩短，从原本的 12 秒减少到了 3 秒。

------

## 5. 设计要点

- 线程安全性：`CountDownLatch` 内部使用 **AQS** 提供的 CAS 操作，确保了计数器递减操作的线程安全性。每个线程在递减计数器时，只有一个线程能够成功递减，避免了竞态条件。

- 可中断性：`await()` 方法支持中断。如果等待线程在 `await()` 方法中被中断，将抛出 `InterruptedException`。这为开发者提供了更高的灵活性，可以处理线程中断的特殊情况。

- 可扩展性：`CountDownLatch` 适用于简单的任务同步，但它无法重置计数器。如果需要重置计数器的功能，可以使用 `CyclicBarrier` 或 `Semaphore`。

## 6. 同步机制详解

### 6.1 共享模式实现

### 6.2 完整执行流程

1. 初始化：设置初始计数
2. `countDown()` 递减计数
3. 当计数归零时，唤醒所有等待线程

## 7. 总结

通过对 `CountDownLatch` 的深入分析，我们可以看到它在并发编程中的巨大作用。`CountDownLatch` 可以帮助我们在多个并发任务完成后，统一返回结果，从而避免了串行执行时的性能瓶颈。合理使用 `CountDownLatch` 能显著提升程序的执行效率与可维护性。

> [!IMPORTANT] 
>`CountDownLatch` 是一种非常实用的并发工具，但它只适合在计数器归零后执行某些操作的场景，不支持计数器重置。如果需要更多功能，请考虑使用其他同步工具。

### 参考

CountDownLatch http://47.120.45.50:4000/posts/countDownLactch.html