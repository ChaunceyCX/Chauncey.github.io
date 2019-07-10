- 实现Runnable接口,重写run()方法
    ```java
    //Runnable只是用来修饰所执行的任务的,它不是一个线程对象,想要启动一个Runnable任务必须将它放到线程对象里面
    public class MyThread implements Runnable {
        @Override
        public void run() {
            //do sth
        }

        public static void main(String[] args) {
            Thread t1 = new Thread(new MyThread());
            t1.start();
        }
    }
    ```

    - 匿名内部类创建线程对象

    ```java
    public static void main(String[] args) {
        //创建无参数线程对象
        new Thread() {
            @Override
            public void run() {
                //do sth
            }
        }.start();

        //创建带线程任务的线程对象
        new Thread( new Runnable() {
            @Override
            public void run() {
                //do sth
            }
        }).start();
    }
    ```

    - 创建带返回值的线程

    ```java
    public class MyThread implements Callable {
        @Override
        public Object call() throws Exception {
            int result = 1;
            //do sth
            return result;
        }

        public static void main(String[] args) {
            MyThread t1 = new MyThread();
            FutureTask<Integer> task = new FutureTask<Integer>(t1);
            Thread thread = new Thread(task);
            thread.start();

            Integer result = task.get();
        }
    }
    ```

    - 定时器Timer创建

    ```java
    public void main(String[] args) {
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                //do sth
            }
        },0,1000); //延迟0,周期1s
    }
    ```

    - 线程池创建线程

    ```java
    public static void main(String[] args) {
        //创建一个有十个线程的线程池
        ExecutorService threadPool = Executors.newFixedThreadPool(10);
        long threadPoolUseTime = System.currentTimeMillis();
        for (int i=0; i<10; i++) {
            threadPool.execute(new Runnable() {
                @Override
                public void run() {
                    //do sth
                }
            });
        }
        long threadPoolUseTime1 = System.currentTimeMills();
        //多线程用时
        threadPoolUserTime1-threadPoolUseTime;
        //销毁线程池
        threadPool.shutdown();
    }
    ```

    - 利用Java8新特性stream实现并发

    ```java
    public static void main(String[] args) {
        List<Integer> values = Arrays.asList(10,20,30,40);
        //parallel: 平行的,并行的
        //所以此行代表并发执行sum();
        int result = values.parallelStream().mapToInt(p->p*2).sum();

        //乱序输出说明是并发执行的
        values.parallelStream().forEach(p-> System.out.println(p));
    }
    ```
2. sleep() 与 wait() 的区别?

    >sleep()会让线程进入阻塞状态,时间结束继续运行(不会释放锁)
    >wait()会让线程释放锁进入等待队列,notify/notifyall后会进入就绪队列
