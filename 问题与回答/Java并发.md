# 基础

## 并行和并发有什么区别？

- **并发**

  - 定义：

    并发是指系统能够处理多个任务的能力，这些任务可以是独立的，也可以是相互交织的。并发并不意味着多个任务同时执行，而是指多个任务在一定时间段内交替进行。

  - 特点：

    - **任务交替执行**：在单核处理器上，并发通过任务的交替执行来实现，即在不同的时间片执行不同的任务。
    - **资源共享**：并发任务通常共享系统资源（如内存、文件等），需要管理资源的访问和同步。
    - **多线程/多进程**：并发通常通过多线程或多进程来实现。

- **并行**

  - 定义：

    并行是指系统能够同时执行多个任务。并行通常需要多核处理器或多台计算机来实现，每个任务在不同的处理单元上同时执行。

  - 特点：

    - **同时执行**：在多核处理器上，并行通过在不同的核心上同时执行多个任务来实现。
    - **任务独立性**：并行任务通常是独立的，彼此之间没有太多的交互。
    - **性能提升**：通过并行执行，可以显著提升系统的处理能力和速度。

## 什么是进程和线程？

在计算机科学中，进程（Process）和线程（Thread）是两个基本的执行单位。

- **进程（Process）**
  - **定义：**进程是操作系统中运行程序的一个实例。每个进程都有自己的地址空间、内存、文件描述符、全局变量等资源，彼此独立。
  - **特点：**
    - **独立性**：进程之间是独立的，一个进程的崩溃不会影响其他进程。
    - **资源隔离**：每个进程拥有自己的资源，如内存和文件句柄，进程之间不能直接共享这些资源。
    - **开销大**：创建和销毁进程的开销较大，因为需要分配和回收大量的系统资源。
    - **通信复杂**：进程之间的通信（IPC，Inter-Process Communication）较为复杂，需要使用管道、消息队列、共享内存等机制。
  - **例子：**
    - 一个浏览器和一个文本编辑器在操作系统中运行时，它们分别是两个独立的进程。
    - 操作系统中的每个服务（如打印服务、网络服务）通常作为一个单独的进程运行。
- **线程（Thread）**
  - **定义：**线程是进程中的一个执行单元。一个进程可以包含多个线程，这些线程共享进程的资源（如内存、文件描述符等），但每个线程有自己的栈和寄存器。
  - **特点：**
    - **共享资源**：同一进程中的线程共享进程的资源，因此线程之间的通信和数据共享更加方便。
    - **轻量级**：线程的创建和销毁开销较小，因为线程之间共享进程的资源，不需要重新分配。
    - **并发执行**：线程可以在多核处理器上并发执行，提高程序的执行效率。
    - **同步问题**：由于线程共享资源，必须小心处理同步问题，避免资源竞争和死锁。
  - **例子：**
    - 在一个浏览器进程中，可以有多个线程分别处理用户界面、网络请求和渲染网页。
    - 在一个服务器进程中，可以有多个线程处理不同的客户端请求。

## 线程的几种创建方式？

1. **继承Thread类**

   通过继承 `Thread` 类并重写其 `run` 方法来定义线程的执行逻辑。

   ```java
   class MyThread extends Thread {
       @Override
       public void run() {
           System.out.println("Thread is running");
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           MyThread thread = new MyThread();
           thread.start(); // 启动线程
       }
   }
   ```

2. **实现Runnable接口**

   这种方式更为灵活，因为Java是单继承的，通过实现 `Runnable` 接口可以避免单继承的限制。

   ```java
   class MyRunnable implements Runnable {
       @Override
       public void run() {
           System.out.println("Thread is running");
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           Thread thread = new Thread(new MyRunnable());
           thread.start(); // 启动线程
       }
   }
   ```

3. **实现Callable接口并使用FuturnTask**

   `Callable` 接口与 `Runnable` 类似，但它可以返回结果并且可以抛出异常。通常与 `FutureTask` 一起使用。

   ```java
   import java.util.concurrent.Callable;
   import java.util.concurrent.ExecutionException;
   import java.util.concurrent.FutureTask;
   
   class MyCallable implements Callable<String> {
       @Override
       public String call() throws Exception {
           return "Thread is running";
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           FutureTask<String> futureTask = new FutureTask<>(new MyCallable());
           Thread thread = new Thread(futureTask);
           thread.start(); // 启动线程
   
           try {
               // 获取线程执行结果
               String result = futureTask.get();
               System.out.println(result);
           } catch (InterruptedException | ExecutionException e) {
               e.printStackTrace();
           }
       }
   }
   ```

4. **使用线程池**

   通过 `ExecutorService` 创建并管理线程池，这是一种更为高级的方式，可以更有效地管理线程的生命周期和资源。

   ```java
   import java.util.concurrent.ExecutorService;
   import java.util.concurrent.Executors;
   
   public class Main {
       public static void main(String[] args) {
           ExecutorService executorService = Executors.newFixedThreadPool(5);
   
           for (int i = 0; i < 10; i++) {
               executorService.submit(new Runnable() {
                   @Override
                   public void run() {
                       System.out.println("Thread is running");
                   }
               });
           }
   
           executorService.shutdown(); // 关闭线程池
       }
   }
   ```

5. **使用Lambda表达式**

   ```java
   public class Main {
       public static void main(String[] args) {
           Thread thread = new Thread(() -> System.out.println("Thread is running"));
           thread.start(); // 启动线程
       }
   }
   ```

## 调用 start()方法时会执行 run()方法，那怎么不直接调用 run()方法？

- **`start()` 方法**：
  - `start()` 方法是 `Thread` 类中的一个方法，当你调用 `start()` 方法时，它会启动一个新的线程，并且在这个新的线程中会自动调用 `run()` 方法。这意味着 `run()` 方法中的代码会在新的线程中执行。
  - `start()` 方法会做一些底层的工作，比如为新线程分配资源、设置线程状态等。
- **`run()` 方法**：
  - `run()` 方法是 `Thread` 类和 `Runnable` 接口中的一个方法，它包含了线程执行的具体代码。
  - 直接调用 `run()` 方法不会启动一个新的线程，`run()` 方法中的代码会在当前线程中执行。

## 线程有哪些方法？

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-6.png" alt="三分恶面渣逆袭：线程常用调度方法" style="zoom: 67%;" />

- `wait()`：当一个线程 A 调用一个共享变量的 `wait()` 方法时，线程 A 会被阻塞挂起，直到发生下面几种情况才会返回 ：
  - 线程 B 调用了共享对象 `notify()`或者 `notifyAll()` 方法；
  - 其他线程调用了线程 A 的 `interrupt()` 方法，线程 A 抛出 InterruptedException 异常返回
- `wait(long timeout)`：这个方法相比 `wait()` 方法多了一个超时参数，它的不同之处在于，如果线程 A 调用共享对象的 `wait(long timeout)`方法后，没有在指定的 timeout 时间内被其它线程唤醒，那么这个方法还是会因为超时而返回。
- `wait(long timeout, int nanos)`，其内部调用的是 `wait(long timout)` 方法。
- `notify()`：一个线程 A 调用共享对象的 `notify()` 方法后，会唤醒一个在这个共享变量上调用 wait 系列方法后被挂起的线程。一个共享变量上可能会有多个线程在等待，具体唤醒哪个等待的线程是随机的。
- `notifyAll()`：不同于在共享变量上调用 `notify()` 方法会唤醒被阻塞到该共享变量上的一个线程，notifyAll 方法会唤醒所有在该共享变量上调用 wait 系列方法而被挂起的线程。
- `join()`：如果一个线程 A 执行了 `thread.join()`，当前线程 A 会等待 thread 线程终止之后才从 `thread.join()` 返回。
- `sleep(long millis)`：Thread 类中的静态方法，当一个执行中的线程 A 调用了 Thread 的 sleep 方法后，线程 A 会暂时让出指定时间的执行权。但是线程 A 所拥有的监视器资源，比如锁，还是持有不让出的。指定的睡眠时间到了后该方法会正常返回，接着参与 CPU 的调度，获取到 CPU 资源后就可以继续运行。
- `yield()`：Thread 类中的静态方法，当一个线程调用 yield 方法时，实际是在暗示线程调度器，当前线程请求让出自己的 CPU，但是线程调度器可能会“装看不见”忽略这个暗示。
- `void interrupt()`：中断线程，例如，当线程 A 运行时，线程 B 可以调用线程 `interrupt()` 方法来设置线程的中断标志为 true 并立即返回。设置标志仅仅是设置标志, 线程 B 实际并没有被中断，会继续往下执行。
- `boolean isInterrupted()` 方法： 检测当前线程是否被中断。
- `boolean interrupted()` 方法： 检测当前线程是否被中断，与 isInterrupted 不同的是，该方法如果发现当前线程被中断，则会清除中断标志。

## 线程的生命周期？

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-7.png" alt="三分恶面渣逆袭：Java线程状态变化" style="zoom: 67%;" />

1. **新建状态（New）**：

   - 线程对象被创建，但还没有调用 `start()` 方法。

   - 代码示例：

     ```java
     Thread thread = new Thread();
     ```

2. **就绪状态（Runnable）**：

   - 线程对象调用了 `start()` 方法，线程进入就绪队列，等待CPU调度。

   - 代码示例：

     ```java
     thread.start();
     ```

   - 在这个状态下，线程可能正在运行，也可能等待操作系统的调度。

3. **运行状态（Running）**：

   - 线程获得CPU时间片，开始执行run()方法中的代码。
   - 线程从就绪状态被操作系统调度到运行状态。

4. **阻塞状态（Blocked）**：

   - 线程因为某种原因（如等待I/O操作、锁等）进入阻塞状态，暂时停止运行，等待条件满足后重新进入就绪状态。

   - 代码示例：

     ```java
     synchronized (lock) {
         lock.wait();
     }
     ```

5. **等待状态（Waiting）**：

   - 线程进入一种特殊的阻塞状态，等待其他线程显式地唤醒。

   - 代码示例：

     ```java
     synchronized (lock) {
         lock.wait();
     }
     ```

6. **超时等待状态（Timed Waiting）**：

   - 线程进入一种特殊的等待状态，但可以在指定时间后自动返回到就绪状态。

   - 代码示例：

     ```java
     Thread.sleep(1000); // 线程休眠1秒
     synchronized (lock) {
         lock.wait(1000); // 等待1秒
     }
     ```

7. **终止状态（Terminated）**：

   - 线程执行完了 `run()` 方法或者因为异常退出，线程生命周期结束。

   - 代码示例：

     ```java
     class MyThread extends Thread {
         public void run() {
             // 线程执行的代码
         }
     }
     MyThread thread = new MyThread();
     thread.start(); // 线程开始运行
     // 线程执行完run()方法后进入终止状态
     ```

## 什么是线程上下文切换？

线程上下文切换（Thread Context Switch）是指操作系统在多线程环境下将CPU从一个线程切换到另一个线程的过程。在这个过程中，操作系统需要保持当前线程的状态，并恢复即将运行的线程的状态，以确保线程在被切换回来的时候能够继续执行。

**线程上下文切换的步骤**

1. **保存当前线程的状态**：
   - 将当前线程的寄存器、程序计数器（Program Counter）、栈指针（Stack Pointer）等信息保存到线程控制块（Thread Control Block, TCB）中。
2. **选择下一个线程**：
   - 操作系统的调度器（Scheduler）根据调度算法选择下一个要运行的线程。
3. **恢复下一个线程的状态**：
   - 从选定线程的TCB中恢复寄存器、程序计数器、栈指针等信息。
4. **切换到下一个线程**：
   - CPU开始执行下一个线程的代码。

**线程上下文切换的开销**

线程上下文切换是有开销的，因为它涉及到保存和恢复寄存器状态、内存管理信息等。频繁的上下文切换会导致系统性能下降，主要原因包括：

- **CPU开销**：保存和恢复线程状态需要CPU时间。
- **缓存失效**：上下文切换可能导致CPU缓存失效（Cache Miss），需要重新加载数据。
- **内存开销**：需要维护线程的状态信息。

**线程上下文切换的原因**

线程上下文切换可以由多种原因引起：

1. **时间片用完**：在时间片轮转调度算法中，每个线程只能运行固定的时间片，当时间片用完后，必须切换到下一个线程。
2. **I/O操作**：线程在等待I/O操作完成时会被阻塞，此时操作系统会切换到其他线程。
3. **同步原语**：线程在等待锁、信号量等同步原语时会被阻塞，导致上下文切换。
4. **优先级调度**：高优先级线程出现时，操作系统可能会切换到高优先级线程。

## 什么是守护线程？

守护线程（Daemon Thread）是Java中的一种特殊类型的线程，它在后台运行，用于执行一些辅助性任务或服务。当所有的非守护线程（用户线程）都结束时，Java虚拟机（JVM）会自动退出，不管守护线程是否还在运行。

**特点**

1. **后台运行**：守护线程在后台执行，通常用于执行一些不重要的任务，如垃圾回收、日志记录等。
2. **不阻止JVM退出**：当所有的非守护线程都结束时，JVM会退出，即使守护线程还在运行。
3. **生命周期依赖于用户线程**：守护线程的生命周期依赖于用户线程，当所有用户线程结束时，守护线程也会终止。

**创建守护线程**

在Java中，可以通过调用 `Thread` 类的 `setDaemon(true)` 方法将一个线程设置为守护线程。需要注意的是，必须在启动线程之前调用 `setDaemon(true)` 方法，否则会抛出 `IllegalThreadStateException` 异常。

**示例**

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(new DaemonTask(), "Daemon-Thread");
        daemonThread.setDaemon(true); // 设置为守护线程
        daemonThread.start();

        Thread userThread = new Thread(new UserTask(), "User-Thread");
        userThread.start();
    }
}

class DaemonTask implements Runnable {
    @Override
    public void run() {
        while (true) {
            try {
                System.out.println(Thread.currentThread().getName() + " is running.");
                Thread.sleep(1000); // 模拟一些后台任务
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class UserTask implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            try {
                System.out.println(Thread.currentThread().getName() + " is running.");
                Thread.sleep(500); // 模拟一些用户任务
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(Thread.currentThread().getName() + " has finished.");
    }
}
```

在这个示例中，`DaemonTask` 是一个守护线程，它会一直在后台运行，打印消息。而 `UserTask` 是一个用户线程，它会运行5次，然后结束。当所有的用户线程（即 `UserTask`）结束时，JVM会退出，不管守护线程（即 `DaemonTask`）是否还在运行。

**守护线程的应用场景**

1. **垃圾回收**：JVM中的垃圾回收器线程就是一个典型的守护线程。
2. **日志记录**：后台记录日志的线程通常也会设置为守护线程。
3. **监控和管理**：一些后台监控和管理任务可以使用守护线程来实现。

**注意事项**

1. **不要依赖守护线程完成重要任务**：由于守护线程在所有用户线程结束后会被强制终止，因此不应该依赖守护线程来完成任何重要的任务。
2. **守护线程的优先级**：守护线程的优先级通常较低，因为它们主要用于后台任务

## 线程间有哪些通信方式？

1. **共享对象或变量**

   线程可以通过共享对象和变量来进行通信。多线程可以访问和修改共享对象的状态，从而实现通信。然而，这种方式需要特别注意同步问题，以避免线程安全问题。

   ```java
   class SharedResource {
       private int value;
   
       public synchronized void setValue(int value) {
           this.value = value;
       }
   
       public synchronized int getValue() {
           return value;
       }
   }
   
   public class SharedObjectExample {
       public static void main(String[] args) {
           SharedResource sharedResource = new SharedResource();
   
           Thread writer = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   sharedResource.setValue(i);
                   System.out.println("Writer set value to " + i);
                   try {
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           Thread reader = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   System.out.println("Reader read value: " + sharedResource.getValue());
                   try {
                       Thread.sleep(150);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           writer.start();
           reader.start();
       }
   }
   ```

2. **`wait()`和`notify()/notifyAll()`方法**

   Java的 `Object` 类提供了 `wait()`、`notify()` 和 `notifyAll()` 方法，这些方法可以用来实现线程间的协调。

   ```java
   class SharedResource {
       private int value;
       private boolean available = false;
   
       public synchronized void setValue(int value) {
           while (available) {
               try {
                   wait();
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
           this.value = value;
           available = true;
           notifyAll();
       }
   
       public synchronized int getValue() {
           while (!available) {
               try {
                   wait();
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
           available = false;
           notifyAll();
           return value;
       }
   }
   
   public class WaitNotifyExample {
       public static void main(String[] args) {
           SharedResource sharedResource = new SharedResource();
   
           Thread writer = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   sharedResource.setValue(i);
                   System.out.println("Writer set value to " + i);
                   try {
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           Thread reader = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   System.out.println("Reader read value: " + sharedResource.getValue());
                   try {
                       Thread.sleep(150);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           writer.start();
           reader.start();
       }
   }
   ```

3. `java.util.concurrent`包

   Java的 `java.util.concurrent` 包提供了更多高级的并发工具，如 `CountDownLatch`、`CyclicBarrier`、`Semaphore`、`Exchanger` 和 `BlockingQueue` 等。

   `BlockingQueue`

   `BlockingQueue` 是一个线程安全的队列，支持阻塞的插入和移除操作。

   ```java
   import java.util.concurrent.ArrayBlockingQueue;
   import java.util.concurrent.BlockingQueue;
   
   public class BlockingQueueExample {
       public static void main(String[] args) {
           BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);
   
           Thread producer = new Thread(() -> {
               for (int i = 0; i < 10; i++) {
                   try {
                       queue.put(i);
                       System.out.println("Produced: " + i);
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           Thread consumer = new Thread(() -> {
               for (int i = 0; i < 10; i++) {
                   try {
                       int value = queue.take();
                       System.out.println("Consumed: " + value);
                       Thread.sleep(150);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           producer.start();
           consumer.start();
       }
   }
   ```

   `CountDownLatch`

   `CountDownLatch` 允许一个或多个线程等待其他线程完成一组操作。

   ```java
   import java.util.concurrent.CountDownLatch;
   
   public class CountDownLatchExample {
       public static void main(String[] args) {
           int numThreads = 3;
           CountDownLatch latch = new CountDownLatch(numThreads);
   
           for (int i = 0; i < numThreads; i++) {
               new Thread(new Worker(latch)).start();
           }
   
           try {
               latch.await(); // 等待所有线程完成
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
   
           System.out.println("All workers are done.");
       }
   }
   
   class Worker implements Runnable {
       private final CountDownLatch latch;
   
       Worker(CountDownLatch latch) {
           this.latch = latch;
       }
   
       @Override
       public void run() {
           System.out.println(Thread.currentThread().getName() + " is working.");
           try {
               Thread.sleep(1000); // 模拟工作
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           latch.countDown(); // 完成工作，减少计数
       }
   }
   ```

   

4. `java.util.concurrent.locks`包

   `ReentrantLock` 和 `Condition` 提供了更灵活的锁和条件变量机制。

   ```java
   import java.util.concurrent.locks.Condition;
   import java.util.concurrent.locks.Lock;
   import java.util.concurrent.locks.ReentrantLock;
   
   class SharedResource {
       private int value;
       private boolean available = false;
       private final Lock lock = new ReentrantLock();
       private final Condition condition = lock.newCondition();
   
       public void setValue(int value) {
           lock.lock();
           try {
               while (available) {
                   condition.await();
               }
               this.value = value;
               available = true;
               condition.signalAll();
           } catch (InterruptedException e) {
               e.printStackTrace();
           } finally {
               lock.unlock();
           }
       }
   
       public int getValue() {
           lock.lock();
           try {
               while (!available) {
                   condition.await();
               }
               available = false;
               condition.signalAll();
               return value;
           } catch (InterruptedException e) {
               e.printStackTrace();
               return -1;
           } finally {
               lock.unlock();
           }
       }
   }
   
   public class LockConditionExample {
       public static void main(String[] args) {
           SharedResource sharedResource = new SharedResource();
   
           Thread writer = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   sharedResource.setValue(i);
                   System.out.println("Writer set value to " + i);
                   try {
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           Thread reader = new Thread(() -> {
               for (int i = 0; i < 5; i++) {
                   System.out.println("Reader read value: " + sharedResource.getValue());
                   try {
                       Thread.sleep(150);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
               }
           });
   
           writer.start();
           reader.start();
       }
   }
   ```

**总结**

不同的线程间通信方式适用于不同的场景和需求。在选择具体的通信方式时，需要根据具体的应用场景、并发需求和性能要求来进行权衡和选择。

## sleep()和wait()的区别？

`sleep` 和 `wait` 都是用于线程控制的方法，但它们有一些关键的区别。以下是它们的主要区别：

**`sleep` 方法**

- **类**: `Thread`
- **目的**: 使当前线程暂停执行一段时间。
- **同步**: 不需要在同步块中使用。
- **锁**: `sleep` 不会释放锁。
- **唤醒**: `sleep` 方法在指定的时间到达后自动唤醒线程。
- **异常**: `sleep` 方法会抛出 `InterruptedException` 异常。

**`wait` 方法**

- **类**: `Object`
- **目的**: 使当前线程等待，直到被其他线程唤醒。
- **同步**: 必须在同步块或同步方法中使用。
- **锁**: `wait` 会释放锁，以便其他线程可以进入同步块或同步方法。
- **唤醒**: `wait` 方法需要被其他线程通过 `notify` 或 `notifyAll` 方法来唤醒。
- **异常**: `wait` 方法会抛出 `InterruptedException` 异常。

# 线程池

## 什么是线程池？

线程池是一种管理和复用线程的机制，用于提升并发程序的性能和效率。线程池通过维护一组预先创建的线程来处理任务，从而减少线程创建和销毁的开销。

## 线程池解决什么问题？

线程池解决的主要就是线程的管理问题。在并发环境下，程序需要执行的任务，需要调度的资源都是不确定的，线程池采用池化思想，就线程资源统一管理。

- 减少线程创建和销毁的开销
- 提升资源利用率
- 简化并发编程
- 控制并发数量，避免资源耗尽
- 任务调度与执行

## 线程池的总体设计？

Java中的线程池核心实现类是ThreadPoolExecutor，该类的继承关系如下。

<img src="https://p1.meituan.net/travelcube/912883e51327e0c7a9d753d11896326511272.png" alt="图1 ThreadPoolExecutor UML类图" style="zoom: 80%;" />

- Executor是顶层接口，它提供了一种将任务提交与任务执行分离的机制。用户只需提供Runnable对象，即将任务逻辑提交给Executor（执行器），由执行器来完成线程的调配和任务的执行。
- ExecutorService接口扩展了Executor接口
  - 提供管理线程池的方法，比如停止线程池的运行
  - 可以生成 Future 的方法来跟踪一个或多个异步任务的进度
- AbstractExecutorService提供 ExecutorService 接口的默认实现
- ThreadPoolExecutor结合这些特性，用于创建一个线程池，管理工作线程，控制任务的提交与执行。

ThreadPoolExecutor运行机制：

<img src="https://p0.meituan.net/travelcube/77441586f6b312a54264e3fcf5eebe2663494.png" alt="图2 ThreadPoolExecutor运行流程" style="zoom: 80%;" />

线程池的内部构造可以理解为一个生产者/消费者模型：

1. 生产者：
   - 角色：提交任务的客户端代码
   - 行为：创建任务（Runnable 或 Callable）并将其提交到线程池
2. 消费者：
   - 角色：线程池中的工作线程
   - 行为：从任务队列中获取任务并执行
3. 共享缓存区：
   - 角色：线程池内部的任务队列
   - 行为：存储等待执行的任务
4. 详细过程：
   - 生产过程：
     - 客户端代码（生产者）通过 execute() 或 submit() 方法提交任务
     - 如果有空闲线程，任务直接交给空闲线程执行
     - 如果没有空闲线程但未达到最大线程数，创建新线程执行任务
     - 如果线程数达到最大值，任务被放入队列（共享缓冲区）等待
   - 消费过程：
     - 工作线程（消费者）不断从任务队列中获取任务
     - 如果队列为空，线程进入等待状态
     - 获取到任务后，线程执行任务
     - 任务执行完毕后，线程再次尝试从队列获取新任务
   - 平衡机制：
     - 当任务提交速度快于执行速度时，队列会积累任务
     - 当队列满时，可能触发拒绝策略或创建新的线程（如果未达到最大线程数）
     - 当任务执行速度快于提交速度时，多余的线程会逐渐被回收（保留核心线程数）

## 线程池的生命周期？

线程池的运行状态，由线程池内部的变量来维护：是一个AtomicInteger类型，包含与运行状态（runState）与线程数量（workerCount）。32位数，前三位保存runState，后29位保存workerCount。

其生命周期包含以下阶段：

1. 创建阶段
   - 线程池被实例化，但还没有创建任何线程
   - 设置核心参数：核心线程数、最大线程数、keepAliveTime、工作队列等
   - 此时线程池处于 RUNNING 状态
2. 运行阶段（RUNNING）
   - 线程池接受新任务并处理排队的任务
   - 可以根据需求创建新的工作线程，直到达到最大线程数
   - 核心线程会一直存活，非核心线程在空闲超时后会被回收
3. 关闭阶段（SHUTDOWN）
   - 调用 shutdown() 方法进入此阶段
   - 不再接受新任务，但会继续处理已经提交的任务（包括队列中的任务）
   - 当所有任务执行完毕，线程池会进入 TIDYING 状态
4. 停止阶段（STOP）
   - 调用 shutdownNow() 方法进入此阶段
   - 尝试停止所有正在执行的任务
   - 不处理队列中尚未执行的任务
   - 返回未执行的任务列表
5. 整理阶段（TIDYING）
   - 所有任务都已终止，工作线程数量为0
   - 转换到此状态的线程会运行 terminated() 钩子方法
6. 终止阶段（TERMINATED）
   - 线程池彻底终止
   - terminated() 钩子方法执行完毕

生命周期状态转换：

- RUNNING -> SHUTDOWN: 调用 shutdown() 方法
- (RUNNING or SHUTDOWN) -> STOP: 调用 shutdownNow() 方法
- SHUTDOWN -> TIDYING: 当队列和线程池都为空
- STOP -> TIDYING: 当线程池为空
- TIDYING -> TERMINATED: 当 terminated() 钩子方法完成

