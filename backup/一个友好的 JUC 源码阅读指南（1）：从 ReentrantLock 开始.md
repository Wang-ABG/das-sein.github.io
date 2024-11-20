## 目录

1. [什么是 ReentrantLock？](##什么是-ReentrantLock)
2. [如何使用 ReentrantLock？](##ReentrantLock-和-synchronized-的对比)
4. [适用场景](##适用场景)
5. [源码：ReentrantLock 的设计与实现](##深入源码：ReentrantLock-的设计与实现)
6. [总结](##总结)
7. [学习建议](##学习建议与下一步)

[附录 加锁解锁部分源码及注解](##加锁解锁部分源码及注解)

---

### 什么是 ReentrantLock？

`ReentrantLock` 是 Java 并发包（`java.util.concurrent.locks`）中的锁实现，提供了比 `synchronized` 更灵活的锁机制。其最大特点是允许**显式控制锁的获取和释放**，并支持诸如公平性、中断性等高级功能。

#### 核心特点

1. **可重入性**  
   和 `synchronized` 类似，同一线程可以多次获取同一把锁，不会发生死锁。
2. **公平锁与非公平锁**  
   - **公平锁**：按请求顺序分配锁，确保线程公平竞争，适用于对公平性要求较高的场景。  
   - **非公平锁**：默认模式，允许线程“插队”，以提升性能。
3. **中断与超时支持**  
   在等待锁时，支持响应中断或设置超时时间，提供更强的灵活性。
4. **条件变量支持**  
   配合 `Condition`，实现线程间复杂的通信机制。



### ReentrantLock 和 synchronized 的对比

| 特性             | `ReentrantLock`                | `synchronized`       |
| ---------------- | ------------------------------ | -------------------- |
| **灵活性**       | 高：可中断、可超时、支持公平锁 | 低：无中断与超时机制 |
| **性能**         | 高（非公平锁优先抢占）         | 较低                 |
| **实现复杂度**   | 高：需显式控制锁的获取与释放   | 低：隐式控制         |
| **条件变量支持** | 支持多个条件变量               | 仅支持 `wait/notify` |

### 适用场景

1. **复杂并发控制**  
   需要等待锁超时、响应中断，或者多个条件变量的场景。

2. **性能优化**  
   对性能要求较高的场景，非公平锁可以减少线程切换，提高吞吐量。

3. **精细化同步**  
   比如需要部分代码加锁，部分不加锁的情况，`ReentrantLock` 的显式控制更加灵活。

### 如何使用 ReentrantLock？

以下是一个简单的例子，展示如何使用 `ReentrantLock` 进行线程同步：

```java
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockExample {
    private final ReentrantLock lock = new ReentrantLock();

    public void criticalSection() {
        lock.lock();  // 获取锁
        try {
            System.out.println(Thread.currentThread().getName() + " is working");
            Thread.sleep(1000);  // 模拟业务逻辑
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            lock.unlock();  // 确保释放锁
        }
    }

    public static void main(String[] args) {
        ReentrantLockExample example = new ReentrantLockExample();
        for (int i = 0; i < 5; i++) {
            new Thread(example::criticalSection).start();
        }
    }
}
```

运行结果表明多个线程依次执行，这正是 `ReentrantLock` 显式控制锁的结果。



### 深入源码：ReentrantLock 的设计与实现

`ReentrantLock` 的核心依赖于 **AQS (AbstractQueuedSynchronizer)**，这是 JUC（`java.util.concurrent`）中用于实现锁和其他同步器的基础框架。理解 AQS 是深入学习 `ReentrantLock` 的关键。

#### 构造函数

以下是 `ReentrantLock` 的核心构造代码：

```java
public ReentrantLock() {
    sync = new NonfairSync();  // 默认使用非公平锁
}

public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

- **非公平锁**（默认）：通过 `NonfairSync` 实现，允许线程插队获取锁。
- **公平锁**：通过 `FairSync` 实现，线程按排队顺序获取锁。


### 加锁的核心流程

调用 `lock()` 方法时：

```java
public void lock() {
    sync.lock();
}
```

以非公平锁为例，其实现为：

```java
final void lock() {
    if (compareAndSetState(0, 1)) {  // 尝试直接获取锁
        setExclusiveOwnerThread(Thread.currentThread());
    } else {
        acquire(1);  // 获取失败，进入 AQS 的队列管理
    }
}
```

- **快速路径**：直接尝试通过 CAS 修改状态，若成功，获取锁。  
- **慢速路径**：调用 AQS 的 `acquire()`，将当前线程加入等待队列，按需排队。



#### 公平锁与非公平锁的区别

两者的不同点主要体现在 `tryAcquire` 方法中：

- **非公平锁**：

  ```java
  protected final boolean tryAcquire(int acquires) {
      if (getState() == 0 && compareAndSetState(0, acquires)) {
          setExclusiveOwnerThread(Thread.currentThread());
          return true;
      }
      return false;
  }
  ```

- **公平锁**：

  ```java
  protected final boolean tryAcquire(int acquires) {
      if (getState() == 0 &&
          !hasQueuedPredecessors() &&  // 关键检查：是否有排队线程
          compareAndSetState(0, acquires)) {
          setExclusiveOwnerThread(Thread.currentThread());
          return true;
      }
      return false;
  }
  ```

公平锁需要先检查队列，确保无其他线程等待。



### 总结

在 Java 并发编程中，`ReentrantLock` 提供了比 `synchronized` 更灵活的锁机制。然而，当我们阅读其源码时，会发现加锁与解锁的逻辑似乎比想象中复杂得多。但值得注意的是，这些复杂性并非完全由 `ReentrantLock` 自身实现，而是大部分依赖于背后的 **AQS (AbstractQueuedSynchronizer)** 框架。

`ReentrantLock` 本质上只是一个包装器，大量核心逻辑是由 AQS 提供的`ReentrantLock` 仅需实现 AQS 中暴露的少量方法，其余功能则完全依赖于 AQS 的默认实现。

### 学习建议与下一步

在阅读 `ReentrantLock` 源码时，你可能会发现逻辑相对复杂。这主要是因为其大量功能依赖于 AQS。如果你尚未完全掌握，可以选择暂时跳过细节，专注于整体架构。理解 AQS 后，回头再看 `ReentrantLock`，一切都会豁然开朗。

下一步，我们将深入学习 **AQS**，剖析其同步队列与状态管理机制。AQS 是 JUC 的核心，掌握它，将为后续理解 `ReentrantLock` 等工具奠定坚实基础。

> **学习建议**：不要急于掌握所有细节，允许自己在初学时“遗忘”。专注于框架设计思想，你会更轻松地突破理解瓶颈。

### 加锁解锁部分源码及注解

附录1 ReentrantLock加锁操作源码，有细微改动，不影响逻辑

```java
public class ReentrantLock implements Lock, java.io.Serializable {
    // 用于同步的核心组件，定义了公平锁和非公平锁的具体实现
    private final Sync sync;

    // 默认构造器，创建一个非公平锁
    public ReentrantLock() {
        sync = new NonfairSync();
    }

    // 可选择公平或非公平锁的构造器
    public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }

    // 加锁操作，委托给 Sync 的具体实现
    public void lock() {
        sync.lock();
    }
    
    // 其他方法省略...
}

abstract static class Sync extends AbstractQueuedSynchronizer {
    // 定义具体加锁方法，由子类实现
    abstract void lock();

    // 判断当前线程是否独占锁
    protected final boolean isHeldExclusively() {
        return getExclusiveOwnerThread() == Thread.currentThread();
    }
}

static final class NonfairSync extends ReentrantLock.Sync {
    // 非公平锁的加锁方法
    final void lock() {
        // 尝试直接获取锁，如果失败，调用 acquire 进入 AQS 的同步队列
        if (compareAndSetState(0, 1)) {
            setExclusiveOwnerThread(Thread.currentThread());
        } else {
            acquire(1);
        }
    }

    // 尝试获取锁，核心逻辑
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        if (c == 0) { // 如果锁未被占用
            if (compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current); // 设置锁的持有线程
                return true;
            }
        } else if (current == getExclusiveOwnerThread()) { // 当前线程已持有锁
            int nextc = c + acquires;
            if (nextc < 0) // 检查锁的递归次数是否溢出
                throw new Error("Maximum lock count exceeded");
            setState(nextc); // 增加锁的重入计数
            return true;
        }
        return false; // 获取锁失败
    }
}

static final class FairSync extends ReentrantLock.Sync {
    // 公平锁的加锁方法
    final void lock() {
        acquire(1);
    }

    // 公平锁的获取逻辑
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        if (c == 0) { // 如果锁未被占用
            // 判断队列中是否有前置线程，公平锁会让等待的线程优先执行
            if (!hasQueuedPredecessors() && compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current);
                return true;
            }
        } else if (current == getExclusiveOwnerThread()) { // 当前线程已持有锁
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc); // 增加锁的重入计数
            return true;
        }
        return false; // 获取锁失败
    }
}

public abstract class AbstractQueuedSynchronizer extends AbstractOwnableSynchronizer
        implements java.io.Serializable {
    
    // AQS 的加锁入口方法
    public final void acquire(int arg) {
        // 首先尝试直接获取锁，如果失败则进入同步队列等待
        if (!tryAcquire(arg) && acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt(); // 如果线程被中断过，则再次设置中断标志
    }

    // 子类必须实现的尝试加锁方法
    protected boolean tryAcquire(int arg) {
        throw new UnsupportedOperationException();
    }

    // 线程在同步队列中等待直到获取锁
    final boolean acquireQueued(final Node node, int arg) {
        boolean failed = true;
        try {
            boolean interrupted = false;
            for (;;) {//
                final Node p = node.predecessor();
                if (p == head && tryAcquire(arg)) { // 如果是队列头且成功获取锁。注意一下，这里还会调用一次tryAcquire(arg)。
                    setHead(node); // 设置当前节点为头节点
                    p.next = null; // 清空前一个头节点的 next 引用
                    failed = false;
                    return interrupted;
                }
                // 检查是否需要阻塞线程，并在必要时阻塞
                if (shouldParkAfterFailedAcquire(p, node) && parkAndCheckInterrupt())
                    interrupted = true;
            }
        } finally {
            if (failed)
                cancelAcquire(node); // 如果失败，从队列中取消当前节点
        }
    }

    // 将线程包装成 Node 并加入等待队列
    private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        Node pred = tail;
        if (pred != null) {
            node.prev = pred;
            if (compareAndSetTail(pred, node)) { // CAS 设置尾节点
                pred.next = node;
                return node;
            }
        }
        enq(node); // 队列为空时进行初始化
        return node;
    }

    // 确保节点被加入到等待队列中
    private Node enq(final Node node) {
        for (;;) {
            Node t = tail;
            if (t == null) { // 队列未初始化
                if (compareAndSetHead(new Node())) // CAS 初始化头节点
                    tail = head;
            } else {
                node.prev = t;
                if (compareAndSetTail(t, node)) { // CAS 设置尾节点
                    t.next = node;
                    return t;
                }
            }
        }
    }
}

```

附录2 ReentrantLock解锁操作源码，有细微改动，不影响逻辑

```java
// 解锁方法定义在 Lock 接口的 unlock 方法中
public class ReentrantLock implements Lock, java.io.Serializable {

    // 调用具体的 Sync 解锁方法
    public void unlock() {
        sync.release(1); // 尝试释放锁
    }

    abstract static class Sync extends AbstractQueuedSynchronizer {

        // 尝试释放锁
        @Override
        protected final boolean tryRelease(int releases) {
            int c = getState() - releases; // 当前锁状态减去释放量
            if (Thread.currentThread() != getExclusiveOwnerThread())
                throw new IllegalMonitorStateException(); // 非锁持有线程尝试解锁，抛异常
            boolean free = false;
            if (c == 0) { // 如果锁完全释放
                free = true;
                setExclusiveOwnerThread(null); // 清空锁持有线程
            }
            setState(c); // 更新锁状态
            return free;
        }
    }

    // AbstractQueuedSynchronizer 的释放锁流程
    public abstract class AbstractQueuedSynchronizer
        extends AbstractOwnableSynchronizer
        implements java.io.Serializable {

        public final boolean release(int arg) {
            if (tryRelease(arg)) { // 调用实现类的 tryRelease
                Node h = head; // 获取等待队列的头节点
                if (h != null && h.waitStatus != 0) // 如果队列中有线程在等待
                    unparkSuccessor(h); // 唤醒后继节点
                return true; // 锁释放成功
            }
            return false; // 未成功释放锁
        }

        // 唤醒后继节点
        private void unparkSuccessor(Node node) {
            int ws = node.waitStatus;
            if (ws < 0) // 如果头节点状态为 SIGNAL，清除它的状态
                compareAndSetWaitStatus(node, ws, 0);

            Node s = node.next;
            if (s == null || s.waitStatus > 0) { // 如果后继节点为空或已取消
                s = null;
                for (Node t = tail; t != null && t != node; t = t.prev) {
                    if (t.waitStatus <= 0) // 找到一个有效等待的节点
                        s = t;
                }
            }
            if (s != null) // 唤醒找到的后继节点
                LockSupport.unpark(s.thread);
        }
    }
}

```

