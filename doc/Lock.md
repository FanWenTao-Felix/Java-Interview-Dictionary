## 1.ReentrantLock

ReentrantLock继承了Lock接口并实现了在接口中定义的方法，是一个可重入的独占锁。ReentrantLock通过自定义队列同步器（AbstractQueued Sychronized，AQS）来实现锁的获取与释放。

独占锁指该锁在同一时刻只能被一个线程获取，而获取锁的其他线程只能在同步队列中等待；可重入锁指该锁能够支持一个线程对同一个资源执行多次加锁操作。

ReentrantLock支持公平锁和非公平锁的实现。公平指线程竞争锁的机制是公平的，而非公平指不同的线程获取锁的机制是不公平的。

ReentrantLock不但提供了synchronized对锁的操作功能，还提供了诸如可响应中断锁、可轮询锁请求、定时锁等避免多线程死锁的方法。

## 2.ReentrantLock的用法

ReentrantLock有显式的操作过程，何时加锁、何时释放锁都在程序员的控制之下。具体的使用流程是定义一个ReentrantLock，在需要加锁的地方通过lock方法加锁，等资源使用完成后再通过unlock方法释放锁。

ReentrantLock之所以被称为可重入锁，是因为ReentrantLock锁可以反复进入。即允许连续两次获得同一把锁，两次释放同一把锁。将上述代码中的注释部分去掉后，程序仍然可以正常执行。注意，获取锁和释放锁的次数要相同，如果释放锁的次数多于获取锁的次数，Java就会抛出java.lang.IllegalMonitorStateException异常；如果释放锁的次数少于获取锁的次数，该线程就会一直持有该锁，其他线程将无法获取锁资源。

## 3.ReentrantLock如何避免死锁

### 响应中断

在synchronized中如果有一个线程尝试获取一把锁，则其结果是要么获取锁继续执行，要么保持等待。ReentrantLock还提供了可响应中断的可能，即在等待锁的过程中，线程可以根据需要取消对锁的请求。具体的实现代码如下：

```
//如果当前线程未被中断，则获取锁
lock.lockInterruptibly();

//检查当前线程是否持有该锁，如果持有释放该锁
if(lock.isHeldByCurrentThread){
	lock.unlock();
}

Long time=System.currentTimeMillis();
InterruptiblyLock interruptiblyLock=new InterruptiblyLock();
Thread thread1=interruptiblyLock.lock1();
Thread thread2=interruptiblyLock.lock2();
//自旋一段时间，如果等待时间过长，则可能发生死锁等问题，主动中断并释放锁
while(true){
	if(System.currentTimeMillis-time>3000){
		thread2.interrupt();//中断线程1
	}
}
```

### 可轮询锁

通过boolean tryLock()获取锁。如果有可用锁，则获取该锁并返回true，如果无可用锁，则立即返回false。

### 定时锁

通过boolean tryLock(long time,TimeUnit unit)throws InterruptedException获取定时锁。如果在给定的时间内获取到了可用锁，且当前线程未被中断，则获取该锁并返回true。如果在给定的时间内获取不到可用锁，将禁用当前线程，并且在发生以下三种情况之前，该线程一直处于休眠状态。

- 当前线程获取到了可用锁并返回true。
- 当前线程在进入此方法时设置了该线程的中断状态，或者当前线程在获取锁时被中断，则将抛出InterruptedException，并清除当前线程的已中断状态。
- 当前线程获取锁的时间超过了指定的等待时间，则将返回false。如果设定的时间小于等于0，则该方法将完全不等待。

## 4.Lock接口的主要方法

Lock接口的主要方法如下。

- voidlock()：给对象加锁，如果锁未被其他线程使用，则当前线程将获取该锁；如果锁正在被其他线程持有，则将禁用当前线程，直到当前线程获取锁。
- boolean tryLock()：试图给对象加锁，如果锁未被其他线程使用，则将获取该锁并返回true，否则返回false。tryLock()和lock()的区别在于tryLock()只是“试图”获取锁，如果没有可用锁，就会立即返回。lock()在锁不可用时会一直等待，直到获取到可用锁。
- tryLock(long timeout TimeUnit unit)：创建定时锁，如果在给定的等待时间内有可用锁，则获取该锁。
- void unlock()：释放当前线程所持有的锁。锁只能由持有者释放，如果当前线程并不持有该锁却执行该方法，则抛出异常。
- Condition newCondition()：创建条件对象，获取等待通知组件。该组件和当前锁绑定，当前线程只有获取了锁才能调用该组件的await()，在调用后当前线程将释放锁。
- getHoldCount()：查询当前线程保持此锁的次数，也就是此线程执行lock方法的次数。
- getQueueLength()：返回等待获取此锁的线程估计数，比如启动 5个线程，1 个线程获得锁，此时返回4。
- getWaitQueueLength(Condition condition)：返回在Condition条件下等待该锁的线程数量。比如有5 个线程用同一个condition对象，并且这 5 个线程都执行了condition对象的await方法，那么执行此方法将返回5。
- hasWaiters(Condition condition)：查询是否有线程正在等待与给定条件有关的锁，即对于指定的contidion对象，有多少线程执行了condition.await方法。
- hasQueuedThread(Thread thread)：查询给定的线程是否等待获取该锁。
- hasQueuedThreads()：查询是否有线程等待该锁。
- isFair()：查询该锁是否为公平锁。
- isHeldByCurrentThread()：查询当前线程是否持有该锁，线程执行lock方法的前后状态分别是false和true。
- isLock()：判断此锁是否被线程占用。
- lockInterruptibly()：如果当前线程未被中断，则获取该锁。
- tryLock（）：尝试获得锁，仅在调用时锁未被线程占用，获得锁

## 5.tryLock、lock和lockInterruptibly的区别

tryLock、lock和lockInterruptibly的区别如下。

- tryLock若有可用锁，则获取该锁并返回true，否则返回false，不会有延迟或等待；tryLock(long timeout,TimeUnit unit)可以增加时间限制，如果超过了指定的时间还没获得锁，则返回false。
- lock若有可用锁，则获取该锁并返回true，否则会一直等待直到获取可用锁。
- 在锁中断时lockInterruptibly会抛出异常，lock不会。

## 6.读写锁：ReadWriteLock

在Java中通过Lock接口及对象可以方便地为对象加锁和释放锁，但是这种锁不区分读写，叫作普通锁。为了提高性能，Java提供了读写锁。读写锁分为读锁和写锁两种，多个读锁不互斥，读锁与写锁互斥。在读的地方使用读锁，在写的地方使用写锁，在没有写锁的情况下，读是无阻塞的。

如果系统要求共享数据可以同时支持很多线程并发读，但不能支持很多线程并发写，那么使用读锁能很大程度地提高效率；

如果系统要求共享数据在同一时刻只能有一个线程在写，且在写的过程中不能读取该共享数据，则需要使用写锁。

一般做法是分别定义一个读锁和一个写锁，在读取共享数据时使用读锁，在使用完成后释放读锁，在写共享数据时使用写锁，在使用完成后释放写锁。在Java中，通过读写锁的接口java.util.concurrent.locks.ReadWriteLock的实现类ReentrantReadWriteLock来完成对读写锁的定义和使用。具体用法如下：

```
public class SeafCache{
	private final Map<String,Object> cache=new HashMap<String,Object>();
	private final ReentrantReadWriteLock rwLock=new ReentrantReadWriteLock();
	private final Lock readLock=rwLock.readLock();//定义读锁
	private final Lock writeLock=rwLock.writeLock();//定义读锁
	//在读数据时加读锁
	public Object get(String key){
		readLock.lock();
		try{
			return cache.get(key);
		}finally{
			readLock.unlock();
		}
	}
	//在写数据时加写锁
	public Object put(String key,Object value){
		writeLock.lock();
		try{
			return cache.put(key,value);
		}finally{
			writeLock.unlock();
		}
	}
}
```

