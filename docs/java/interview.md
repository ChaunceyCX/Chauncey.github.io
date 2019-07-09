# java面试题整理

## 线程相关

1. sleep() 与 wait() 的区别?

    >sleep()会让线程进入阻塞状态,时间结束继续运行(不会释放锁)
    >wait()会让线程释放锁进入等待队列,notify/notifyall后会进入就绪队列
