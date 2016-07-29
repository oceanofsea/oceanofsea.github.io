---
layout: post
title:  "Java Concurrency"
date:   2016-04-03
categories: Java
excerpt: 
tags: Java multithread concurrency lock
---
* content
{:toc}

## 多线程编程的好处 [^2]
[^2]: http://ifeve.com/java-multi-threading-concurrency-interview-questions-with-answers/

在多线程程序中，多个线程被并发的执行以提高程序的效率，CPU不会因为某个线程需要等待资源而进入空闲状态。多个线程共享堆内存(heap memory)，因此创建多个线程去执行一些任务会比创建多个进程更好。

## Java Thread Lifecycle [^1]

[^1]: http://www.journaldev.com/1044/life-cycle-of-thread-understanding-thread-states-in-java

Below diagram shows different states of thread in java, note that we can create a thread in java and start it but how the thread states change from Runnable to Running to Blocked depends on the OS implementation of thread scheduler and java doesn’t have full control on that.

当我们在Java程序中新建一个线程时，它的状态是New。当我们调用线程的start()方法时，状态被改变为Runnable。线程调度器会为Runnable线程池中的线程分配CPU时间并且讲它们的状态改变为Running。其他的线程状态还有Waiting，Blocked 和Dead

![life cycle of thread in Java]({{ site.url }}/images/Thread-Lifecycle-States.png)


## java 实现multithread的两种方式

Implements Runnable or extends Thread. Because in Java, only extends one class, so implements Runnable is prefered

## sleep

Thread.sleep causes the current thread to suspend execution for a specified period. However, **these sleep times are not guaranteed to be precise**, because they are limited by the facilities provided by the underlying OS. Also, the sleep period can be terminated by interrupts.

## interrupt
An interrupt is an indication to a thread that it should stop what it is doing and do something else. 

> e.g. sleep will throw interrupt exception

## Joins

The join method allows one thread to wait for the completion of another. If t is a Thread object whose thread is currently executing

t.join();

causes the current thread to pause execution until t's thread terminates.

## Synchronization
当多线程去读写shared resources的时候，就需要对resource进行控制以免发生错误。
同时也会引发问题，如starvation 和 live lock。

### implementation

Synchronized method and Synchronized statement

#### Synchronized method

```
public synchronized void increment() {
	c++;
}
```
 When one thread is executing a synchronized method for an object, all other threads that invoke synchronized methods for the same object block (suspend execution) until the first thread is done with the object.

#### Inside Sychonization
Every object has an intrinsic lock. A thread need to acquire the lock of this object before accessing them, and release the lock after done with it. When a thread invokes a synchronized method, it automatically acquires the intrinsic lock for that method's object and releases it when the method returns.

#### Synchronized Statements

Another way to create synchronized code is with synchronized statements. Unlike synchronized methods, synchronized statements must specify the object that provides the intrinsic lock:

```
public void addName(String name) {
    synchronized(this) {
        lastName = name;
        nameCount++;
    }
    nameList.add(name);
}
```

### Atomic Access, Volatile

Reads and writes are atomic for reference variables and for most primitive variables (all types except long and double).
Reads and writes are atomic for all variables declared volatile (including long and double variables).

当我们使用volatile关键字去修饰变量的时候，所以线程都会直接读取该变量并且不缓存它。这就确保了线程读取到的变量是同内存中是一致的。

### Deadlock, Starvation, Livelock
* Deadlock: 竞争资源无法move on，被block了
* starvation: 竞争不到饿死了
* Livelock: 谦虚地一直在退让，没有被block依然无法move on

## Lock object

lock：需要显示指定起始位置和终止位置。一般使用ReentrantLock类做为锁，多个线程中必须要使用一个ReentrantLock类做为对象才能保证锁的生效。且在加锁和解锁处需要通过lock()和unlock()显示指出。所以一般会在finally块中写unlock()以防死锁。[^3]

[^3]:http://blog.csdn.net/natian306/article/details/18504111

synchronized原语和ReentrantLock在一般情况下没有什么区别，但是在非常复杂的同步应用中，请考虑使用ReentrantLock，特别是遇到下面2种需求的时候。
 
1. 某个线程在等待一个锁的控制权的这段时间需要中断
2. 需要分开处理一些wait-notify，ReentrantLock里面的Condition应用，能够控制notify哪个线程
3. 具有公平锁功能，每个到来的线程都将排队等候

就是说Lock更灵活一点。


 
## Reference