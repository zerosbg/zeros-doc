## ReentrantLock非公平锁与公平锁的实现

在文章开始之前，大家复习一遍锁的分类：

ReentrantLock是根据传入的参数来决定是否使用公平锁，默认使用非公平锁：
- 公平锁/非公平锁

    当多个线程来取锁的时候，按照规则排队等锁即为公平锁，不按照规则排队的即为非公平锁, Synchronized就是一个典型的非公平锁，而ReentrantLock
    是根据AQS来实现线程的一个调度达到公平锁与非公平锁的一个切换。
    
- 可重入锁
    
    一个带锁的方法块可以调用另一个带锁的方法块,也就是在执行线程的期间，前后会用到两个锁，可重入锁就有效的避免了前后死锁的情况。

- 独享锁/共享锁

    独享锁是该锁只能被一个线程所持有，共享锁是值该锁可以被多个线程所持有。
    ReentrantLock和Synchronized都是独享锁，而ReadWriteLock中读锁是共享锁，写锁是独享锁
    
- 乐观锁/悲观锁

    悲观锁适合写操作，乐观锁适合读操作。

- 分段锁

    是针对部分数据设置的一种锁，例如我要改变某一个有规则的数据链，我定位在某一块，只需要锁住这一块的数据，就能保证整体的数据一致。
    例如在 CurrentHashMap中，根据hashcode定位数据块，实现分段锁。

- 偏向锁/轻量级锁/重量级锁

- 自旋锁

    自旋锁是采用让当前线程不停地的在循环体内执行实现的，当循环的条件被其他线程改变时 才能进入临界区
    可以参考：http://ifeve.com/java_lock_see1/

大家说到ReentrantLock这个锁，一般情况下第一个想法是它是一个可冲入锁，但是我认为另一个概念公平锁和非公平锁的实现更能体现出它的内涵：

```java
//使用默认的非公平锁
ReentrantLock nonFairReentrantLock = new ReentrantLock();
//构造函数入参传true使用公平锁
ReentrantLock fairReentrantLock = new ReentrantLock(true);
```

判断内容如下：

```java
public ReentrantLock(boolean fair) {
  sync = fair ? new FairSync() : new NonfairSync();
}
```
从上边代码我们看到，通过构造函数中的一个布尔入参实现具体声明公平锁还是非公平锁。

以下是非公平锁的实现，在lock方法和tryAcquire方法中添加compareAndSetState方法，判断当前锁是否被占用，如果没有则当前线程会占用锁，
不会去判断队列中是否有队列是否在等待。以下是代码解释

```java
static final class NonfairSync extends Sync {
    private static final long serialVersionUID = 7316153563782823691L;
    /**
    * Performs lock.  Try immediate barge, backing up to normal
    * acquire on failure.
    */
    final void lock() {
        if (compareAndSetState(0, 1))
            setExclusiveOwnerThread(Thread.currentThread());
        else
            acquire(1);
        }

        protected final boolean tryAcquire(int acquires) {
            return nonfairTryAcquire(acquires);
        }
        
    }

    final boolean nonfairTryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
            if (c == 0) {
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
    }
}

```

以下是公平锁，tryAcquire方法中判断了队列中的是否有线程等待。。

```java
static final class FairSync extends Sync {
    private static final long serialVersionUID = -3000897897090466540L;

    final void lock() {
        acquire(1);
    }

    /**
    * Fair version of tryAcquire.  Don't grant access unless
    * recursive call or no waiters or is first.
    */
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        if (c == 0) {
            if (!hasQueuedPredecessors() &&
                compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current);
                return true;
            }
        }
        else if (current == getExclusiveOwnerThread()) {
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        return false;
    }
}

```
通过重入锁的公平锁与非公平锁的实现，我们看到是依赖于底层AQS来实现的，所以在这简单讲解一下AQS的原理

AQS定义两种资源共享方式：Exclusive(独占，只有一个线程执行)和share(共享，多个线程可同时执行，如Semaphore/CountDownLatch)

在这引用 waterystone博客中的两句总结：

>以ReentrantLock为例，state初始化为0，表示未锁定状态。A线程lock()时，会调用tryAcquire()独占该锁并将state+1。
此后，其他线程再tryAcquire()时就会失败，直到A线程unlock()到state=0（即释放锁）为止，其它线程才有机会获取该锁。
当然，释放锁之前，A线程自己是可以重复获取此锁的（state会累加），这就是可重入的概念。但要注意，获取多少次就要释放多么次，这样才能保证state是能回到零态的。

>再以CountDownLatch以例，任务分为N个子线程去执行，state也初始化为N（注意N要与线程个数一致）。这N个子线程是并行执行的，每个子线程执行完后countDown()一次，state会CAS减1。等到所有子线程都执行完后(即state=0)，会unpark()主调用线程，然后主调用线程就会从await()函数返回，继续后余动作。

总结：ReentrantLock通过构造参数fair来判断是创建公平锁还是非公平锁，底层中的独享锁的实现以及队列等待功能依赖于AQS，
AQS是java中大部分锁的基础，其中可以划分独享和共享，根据volatile字段state实现可冲入。