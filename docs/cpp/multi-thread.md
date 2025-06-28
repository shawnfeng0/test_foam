# Multi Thread

## 多线程编程的文章

信号量的使用技巧： <https://preshing.com/20150316/semaphores-are-surprisingly-versatile/>
包括轻量级的锁实现和轻量级的通知机制的实现。

C++11原子操作： <https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/>

上面两篇文章作者关于C++多线程原语的一些demo: <https://github.com/preshing/cpp11-on-multicore>

## 多发布者多订阅者的消息队列的实现

全网最好的发布订阅实现 <https://github.com/cameron314/concurrentqueue>

## Q&&A

* 使用互斥锁的时候为什么不推荐使用递归锁？

    当使用了递归锁的时候，很可能是你的设计出了问题，或者设置太大的不合理的临界区。
    使用递归锁会让对于代码结构没那么重视，每个临界区的设计可能都没有被设计为最优，这个懒惰会让你的代码设计越来越烂。比如出现那种A锁B锁再A锁的情况，这真的很复杂，不是一个好的设计。

* 为什么推荐使用pthread_cond而不是sem信号量？

    cond的api可以明确设置clock源，使用CLOCK_MONOTONIC单调时钟总是符合我们的期待的，我们想用它等待一段时间，而不是CLOCK_REALTIME的真实时间，因为当外部的realtime时钟被修改后，超时也就不准确了。我们永远不应该使用使用CLOCK_REALTIME作为时钟源的API, 比如sem_xxx类的。还有mutex的timed_wait也是的。

    参考讨论：
    
    stackoverflow上面的讨论：<https://stackoverflow.com/questions/14248033/clock-monotonic-and-pthread-mutex-timedlock-pthread-cond-timedwait>
    android的bionic库，强制把基于CLOCK_REALTIME的时间改成基于CLOCK_MONOTONIC的时间以免受realtime被外部修改后导致超时时间不准确问题的影响：<https://android.googlesource.com/platform/bionic/+/refs/tags/android-13.0.0_r49/libc/bionic/bionic_futex.cpp>
