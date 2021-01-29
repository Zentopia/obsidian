关于 [[monitorenter]] 和 [[monitorexit]] 的作用，我们可以抽象地理解为每个[[锁对象]]，拥有一个锁计数器和一个指向持有该锁线程的指针。

当执行 [[monitorenter]] 时，如果目标锁对象的计数器为 0，那么说明它没有被其它线程所持有。在这个情况下，JVM 会将该[[锁对象]]的持有线程设置为当前线程，并且将其计数器加 1。

在目标锁对象的计数器不为 0 的情况下，如果锁对象的持有线程是当前线程，那么 JVM 可以将其计数器加 1，否则需要等待，直到持有线程释放该锁。

当执行 [[monitorexit]] 时，JVM 需要将锁对象的计数器减 1。当计数器减为 0 时，那便代表该锁已经被释放掉了。

只所以采用计数器的方式，是为了允许同一个线程重复获取同一把锁。