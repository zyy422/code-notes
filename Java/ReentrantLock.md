# ReentrantLock和Synchronized的区别
- ReentrantLock基于JDK实现，Synchronized基于JDK源码实现
- 性能差异不大：synchnorized借鉴了CAS技术，都是尝试在用户态就把加锁的问题进行解决
- ReentrantLock可以直接阅读源码，更好
- ReentrantLock提供了一种终端等待锁的线程机制
- ReentrantLock可以指定为公平或者是非公平锁
- ReentrantLock提供了一个Condition条件，实现分组唤醒需要唤醒的线程
- ReentrantLock提供了一种能够终端等待所的线程机制

## 轻量级锁思想
Synchronized为重量级锁，需要线程来进行内核态和用户态的切换。线程A -〉线程B,线程A需要保存当前线程，线程B也需要保存当前现场。

**缺点：** 耗费系统资源

ReentrantLock: 轻量级锁，基于cas和volatile，不需要线程切换，获取锁时觉得自己肯定能成功，属于**乐观锁**


## 公平和非公平
Synchronzied: 仅有非公平锁

ReentrantLock: 默认非公平，也可以构造公平锁(**是否允许插队的区别**)
## 可终端和不可中断
Synchronzied：不可中断

ReentrantLock：可中断和不可中断

## 条件队列
Synchronzied：仅有一个等待队列

ReentrantLock：可以对应多个等待队列