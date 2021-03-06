   #### CAS简介
    CAS(compare-and-swap)直译即比较并交换，提供原子化的读改写能力，是Java 并发中所谓 lock-free 机制的基础。
    CAS的思想很简单：三个参数，一个当前内存值V、旧的预期值A、即将更新的值B，当且仅当预期值A和内存值V相同时，
    将内存值修改为B并返回true，否则什么都不做，并返回false。
    可能会有面试官问 CAS 底层是如何实现的，在JAVA中,CAS通过调用C++库实现，由C++库再去调用CPU指令集。
    不同体系结构中，cpu指令还存在着明显不同。比如，x86 CPU 提供 cmpxchg 指令；而在精简指令集的体系架构中，
    （如“load and reserve”和“store conditional”）实现的，在大多数处理器上 CAS 都是个非常轻量级的操作，
    这也是其优势所在。
    
   #### CAS的缺点有以下几个方面：
    1.ABA问题
        如果某个线程在CAS操作时发现，内存值和预期值都是A，就能确定期间没有线程对值进行修改吗？
        答案未必，如果期间发生了 A -> B -> A 的更新，仅仅判断数值是 A，可能导致不合理的修改操作。针对这种情况，
        Java 提供了 AtomicStampedReference 工具类，通过为引用建立类似版本号（stamp）的方式，来保证 CAS 的正确性。
    2.循环时间长开销大
        CAS中使用的失败重试机制，隐藏着一个假设，即竞争情况是短暂的。大多数应用场景中，
        确实大部分重试只会发生一次就获得了成功。但是总有意外情况，所以在有需要的时候，还是要考虑限制自旋的次数，
        以免过度消耗 CPU。
    3.只能保证一个共享变量的原子操作
    
    作者：一路花开_8fab
    链接：https://www.jianshu.com/p/cea1f9619e8f
    来源：简书
    简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。