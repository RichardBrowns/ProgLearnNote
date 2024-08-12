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
- `join()`：使用 `thread.join()` 的目的是确保主线程等待所有子线程完成执行，然后再继续执行后续的代码。
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

# ThreadLocal

## 什么是ThreadLocal？

`ThreadLocal` 是 Java 中提供的一种用于实现线程局部变量的机制。通过 `ThreadLocal`，每个线程都可以拥有自己独立的变量副本，从而避免了多线程环境下的变量共享问题。这对于一些需要在多线程环境下保持独立状态的场景非常有用。

**基本概念**

`ThreadLocal` 提供了一种简单的方式，使每个线程都能独立地存取变量，而不需要显式地同步。每个线程都可以通过 `ThreadLocal` 对象访问到自己独有的变量副本。

**使用方法**

以下是 `ThreadLocal` 的基本使用方法：

1. 创建`ThreadLocal`实例：

   ```java
   private static final ThreadLocal<Integer> threadLocalVariable = new ThreadLocal<>();
   ```

2. 设置值：

   ```java
   threadLocalVariable.set(123);
   ```

3. 获取值：

   ```java
   Integer value = threadLocalVariable.get();
   ```

4. 删除值：

   ```java
   threadLocalVariable.remove();
   ```

**示例代码**

```java
public class ThreadLocalExample {
    private static final ThreadLocal<Integer> threadLocalVariable = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        Runnable task = () -> {
            int currentValue = threadLocalVariable.get();
            System.out.println(Thread.currentThread().getName() + " initial value: " + currentValue);

            threadLocalVariable.set(currentValue + 1);
            System.out.println(Thread.currentThread().getName() + " incremented value: " + threadLocalVariable.get());
        };

        Thread thread1 = new Thread(task, "Thread-1");
        Thread thread2 = new Thread(task, "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```

在这个示例中，每个线程都有自己的 `threadLocalVariable` 副本，互不干扰。

**适用场景**

`ThreadLocal` 适用于以下场景：

1. **数据库连接管理**：在一个多线程的应用程序中，每个线程可能需要一个独立的数据库连接。使用 `ThreadLocal` 可以确保每个线程都有自己的数据库连接实例，从而避免了连接被多个线程共享的问题。
2. **避免显式同步**：当不希望使用显式的同步机制来保证线程安全时，`ThreadLocal` 提供了一种简洁的解决方案。
3. **线程上下文信息**：在某些框架中，`ThreadLocal` 被用来存储线程上下文信息，比如用户身份信息、事务上下文等。

**注意事项**

虽然 `ThreadLocal` 提供了一种简便的方式来处理线程局部变量，但在使用时需要注意以下几点：

1. **内存泄漏**：`ThreadLocal` 变量在不再使用时应该显式调用 `remove` 方法以防止内存泄漏，尤其是在使用线程池时。
2. **用户身份认证**：在一个 Web 应用中，每个线程可能需要处理不同用户的请求。使用 `ThreadLocal` 可以存储每个线程的用户身份信息，从而简化认证和授权的处理。
3. **日期格式化**：在多线程环境中使用 `SimpleDateFormat` 是不安全的，因为它不是线程安全的。使用 `ThreadLocal` 可以为每个线程提供一个独立的 `SimpleDateFormat` 实例。

## ThreadLocal实现原理？

`ThreadLocal` 的实现原理主要涉及以下几个方面：

1. **`ThreadLocal`类**

   `ThreadLocal` 类提供了以下几个主要方法：

   - `get()`: 获取当前线程的本地变量值。
   - `set(T value)`: 设置当前线程的本地变量值。
   - `remove()`: 移除当前线程的本地变量值。

2. **`ThreadLocalMap`**

   `ThreadLocal` 的核心是一个静态内部类 `ThreadLocalMap`，它是一个定制化的哈希表，用于存储每个线程的本地变量。

   ```java
   static class ThreadLocalMap {
       static class Entry extends WeakReference<ThreadLocal<?>> {
           Object value;
   
           Entry(ThreadLocal<?> k, Object v) {
               super(k);
               value = v;
           }
       }
   
       private Entry[] table;
       private int size;
   
       // 其他方法和内部逻辑
   }
   ```

3. **每个线程都有一个`ThreadLocalMap`**

   每个线程都有一个 `ThreadLocalMap` 实例，用于存储该线程的所有 `ThreadLocal` 变量。这个 `ThreadLocalMap` 存储在 `Thread` 类的 `threadLocals` 字段中：

   ```java
   public class Thread implements Runnable {
       ThreadLocal.ThreadLocalMap threadLocals = null;
   }
   ```

4. **`ThreadLocal`的`get`和`set`方法**

   `get`方法

   当调用`ThreadLocal` 的 `get` 方法时，它会从当前线程的 `ThreadLocalMap` 中获取对应的值：

   ```java
   public T get() {
       Thread t = Thread.currentThread();
       ThreadLocalMap map = getMap(t);
       if (map != null) {
           ThreadLocalMap.Entry e = map.getEntry(this);
           if (e != null) {
               @SuppressWarnings("unchecked")
               T result = (T)e.value;
               return result;
           }
       }
       return setInitialValue();
   }
   ```

   `set`方法

   当调用 `ThreadLocal` 的 `set` 方法时，它会将值存储到当前线程的 `ThreadLocalMap` 中：

   ```java
   public void set(T value) {
       Thread t = Thread.currentThread();
       ThreadLocalMap map = getMap(t);
       if (map != null)
           map.set(this, value);
       else
           createMap(t, value);
   }
   ```

5. **内存泄漏问题**

   由于 `ThreadLocalMap` 的 `Entry` 使用了 `WeakReference` 来引用 `ThreadLocal` 对象，当 `ThreadLocal` 对象拥有强引用时，GC 可以回收它。但是，`Entry` 中的 `value` 是强引用，这可能导致内存泄漏问题。因此，使用 `ThreadLocal` 时，建议在使用完后调用 `remove` 方法来清除数据。

   ```java
   public void remove() {
       ThreadLocalMap m = getMap(Thread.currentThread());
       if (m != null)
           m.remove(this);
   }
   ```

**总结**

- `ThreadLocal` 允许每个线程存储和访问自己的变量副本。
- 每个线程都有一个 `ThreadLocalMap` 实例，用于存储所有 `ThreadLocal` 变量。
- `ThreadLocalMap` 是一个定制化的哈希表，使用 `WeakReference` 来引用 `ThreadLocal` 对象。
- 使用 `ThreadLocal` 时需要注意内存泄漏问题，建议在使用完后调用 `remove` 方法。

# Java内存模型

## 什么是Java内存模型？

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/thread/jmm-f02219aa-e762-4df0-ac08-6f4cceb535c2.jpg" alt="深入浅出 Java 多线程：Java内存模型" style="zoom: 50%;" />

Java 内存模型（Java Memory Model, JMM）是 Java 虚拟机规范中的一部分，它定义了在多线程环境下，Java 程序中变量的读取和写入行为。JMM 确定了一个程序中共享变量（如实例字段、静态字段和数组元素）在不同线程之间的可见性和有序性。理解 JMM 对于编写正确的并发程序至关重要。

- **主内存**：所有的变量都存储在主内存中，这是一个共享内存区域。
- **工作内存**：每个线程都有自己的工作内存。线程的工作内存保存了被该线程使用的变量的主内存副本。

线程对变量的所有操作（读、写）都必须在工作内存中进行，而不能直接读写主内存中的变量。线程之间变量值的传递需要通过主内存来完成。

## 原子性、可见性和有序性？

在并发编程中，原子性、可见性和有序性是确保多线程程序正确性的三个重要概念。

1. **原子性（Atomicity）**

   原子性是指一个操作是不可中断的，即使在多个线程同时执行的情况下，一个原子操作一旦开始，就不会被其他线程看到中间状态，要么完全执行，要么完全不执行。

   **例子：**

   - **基本类型的读取和赋值**：例如 `int`、`float` 等类型的读取和赋值操作是原子的。
   - **Atomic 类**：如 `AtomicInteger`、`AtomicLong` 等，提供了一些原子操作方法（如 `getAndIncrement()`、`compareAndSet()`），这些方法在多线程环境下也是原子的。

2. **可见性（Visibility）**

   可见性是指当一个线程修改了某个共享变量的值，其他线程能够立即看到这个修改。Java 内存模型通过一些机制来保证变量的可见性。

   **例子：**

   - **volatile 关键字**：声明为 `volatile` 的变量在被一个线程修改后，其他线程能够立即看到最新的值。`volatile` 变量在被读写时不会被缓存，每次读写都直接从主内存中获取。
   - **synchronized 关键字**：进入和退出同步块（`synchronized` 方法或代码块）会刷新线程的工作内存，确保共享变量的可见性。

3. **有序性（Ordering）**

   有序性是指程序执行的顺序按照代码的顺序进行。编译器和处理器为了优化性能，可能会对指令进行重排序，但这种重排序不会影响单线程程序的执行结果。多线程环境下，重排序可能导致可见性问题。

   **例子：**

   - happens-before 规则：

     Java 内存模型通过一组 happens-before 规则来确保有序性。例如：

     - 程序顺序规则：在一个线程内，按照代码顺序，前面的操作 happens-before 后面的操作。
     - 监视器锁规则：一个 unlock 操作 happens-before 后面对同一个锁的 lock 操作。
     - volatile 变量规则：对一个 volatile 变量的写操作 happens-before 后面对这个变量的读操作。

**总结**

- **原子性**：确保操作不可中断。
- **可见性**：确保一个线程对共享变量的修改能够被其他线程立即看到。
- **有序性**：确保代码执行的顺序按照预期进行。

## 什么是指令重排？

指令重排（Instruction Reordering）是指编译器和处理器在不改变单线程程序语义的前提下，为了优化性能，对指令执行顺序进行调整的过程。这种优化可以提高指令流水线的效率，减少处理器的等待时间，从而提高程序的执行速度。

**为什么需要指令重排？**

1. **提高指令并行度**：现代处理器具有多级流水线和多个执行单元，通过重排指令，可以更好地利用这些硬件资源，提高指令的并行执行效率。
2. **减少等待时间**：某些指令可能需要等待数据或资源，通过重排，可以安排其他不依赖这些数据或资源的指令先执行，减少处理器的空闲时间。

**指令重排的影响**

在单线程环境中，指令重排不会影响程序的正确性，因为编译器和处理器会确保重排后的指令序列与原始序列在逻辑上等价。然而，在多线程环境中，指令重排可能会导致一些难以察觉的并发问题，如可见性问题和竞态条件。

**例子**

```java
int a = 0;
boolean flag = false;

// 线程1
a = 1;
flag = true;

// 线程2
if (flag) {
    System.out.println(a);
}
```

在这个例子中，如果没有指令重排，线程2中的 `if (flag)` 判断为 `true` 时，`a` 的值应该是 `1`。但是由于指令重排的存在，线程1中的两条指令 `a = 1` 和 `flag = true` 可能会被重排，导致 `flag` 被设置为 `true` 之前，`a` 还没有被设置为 `1`。这样，线程2可能会看到 `flag` 为 `true`，但 `a` 的值仍然是 `0`，从而打印出错误的结果。

**如何避免指令重排**

在 Java 中，可以使用一些机制来避免指令重排带来的问题：

1. **volatile 关键字**：声明为 `volatile` 的变量禁止指令重排。对 `volatile` 变量的读写操作会具有内存屏障效果，确保指令的执行顺序。

   ```java
   volatile int a = 0;
   volatile boolean flag = false;
   ```

2. **synchronized 关键字**：进入和退出同步块时，会有内存屏障，确保同步块内的指令不会被重排到同步块外。

   ```java
   synchronized(this) {
       a = 1;
       flag = true;
   }
   ```

3. **原子类**：使用 `java.util.concurrent.atomic` 包中的原子类（如 `AtomicInteger`、`AtomicBoolean`）进行原子操作，也可以避免指令重排。

   ```java
   AtomicInteger a = new AtomicInteger(0);
   AtomicBoolean flag = new AtomicBoolean(false);
   ```

**总结**

指令重排是编译器和处理器为了优化性能而进行的指令顺序调整。在单线程环境中，指令重排不会影响程序的正确性，但在多线程环境中，可能会导致并发问题。通过使用 `volatile` 关键字、`synchronized` 关键字和原子类，可以避免指令重排带来的问题，确保多线程程序的正确性。

## 什么是happens-before？

在并发编程中，理解线程之间的执行顺序和内存可见性是非常重要的。Java内存模型（Java Memory Model, JMM）为此引入了“happens-before”关系，来帮助开发者理解和控制线程之间的交互。

**什么是happens-before？**

"happens-before" 是一种关系，用来确定两个操作在多线程环境中的执行顺序。具体来说，如果操作A happens-before 操作B，那么操作A的结果对操作B是可见的，并且A的执行顺序在B之前。

**happens-before规则**

以下是一些常见的 happens-before 规则：

1. **程序顺序规则**：在一个线程内，按照程序顺序，前面的操作 happens-before 后面的操作。
2. **监视器锁规则**：对一个锁的解锁 happens-before 于随后对这个锁的加锁。
3. **volatile 变量规则**：对一个 volatile 变量的写操作 happens-before 于随后对这个 volatile 变量的读操作。
4. **传递性**：如果 A happens-before B，且 B happens-before C，那么 A happens-before C。
5. **线程启动规则**：主线程 A 启动子线程 B，主线程 A 中的所有操作 happens-before 子线程 B 中的任何操作。
6. **线程终止规则**：子线程 B 中的所有操作 happens-before 主线程 A 检测到子线程 B 已经终止。
7. **线程中断规则**：对线程 interrupt() 方法的调用 happens-before 检测到中断事件的发生。
8. **对象终结规则**：一个对象的构造函数执行完成 happens-before 该对象的 finalize() 方法的开始。

**为什么需要happens-before？**

在多线程编程中，happens-before 关系对于理解和控制线程之间的可见性和顺序性至关重要。它确保了在多线程环境中，某些操作的结果对其他线程是可见的，并且操作的顺序是有保证的。

**例子**

```java
class Example {
    private int a = 0;
    private volatile boolean flag = false;

    public void writer() {
        a = 1;           // 写操作1
        flag = true;     // 写操作2
    }

    public void reader() {
        if (flag) {      // 读操作1
            int i = a;   // 读操作2
            // i 的值是多少？
        }
    }
}
```

在这个例子中：

- 写操作1 `a = 1` happens-before 写操作2 `flag = true`（程序顺序规则）。
- 写操作2 `flag = true` happens-before 读操作1 `if (flag)`（volatile 变量规则）。
- 读操作1 `if (flag)` happens-before 读操作2 `int i = a`（程序顺序规则）。

因此，如果 `reader` 线程看到 `flag` 为 `true`，那么它一定也会看到 `a` 的值为 `1`。

## 什么是as-if-serial？

在编程语言和编译器优化中，“as-if-serial”原则是一个重要的概念。它确保了编译器在进行优化时，不会改变程序的行为和结果。具体来说，“as-if-serial”原则保证了程序的最终结果与按照源代码顺序执行的结果是相同的，即使编译器对代码进行了重排序或其他优化。

**定义**

“as-if-serial”原则规定，无论编译器如何优化代码，程序的行为都必须与未优化的、按照源代码顺序执行的程序相同。换句话说，编译器可以自由地对代码进行重排序、合并、消除等优化，只要这些优化不会改变程序的可观察行为。

**可观察行为**

可观察行为通常包括：

- 程序的输出，例如通过 `printf` 或 `System.out.println` 输出到控制台。
- 对 volatile 变量的读写。
- I/O 操作，例如文件读写、网络通信。
- 其他具有副作用的操作，例如修改全局变量或与外部系统交互。

**例子**

```c
int a = 1;
int b = 2;
int c = a + b;
printf("%d\n", c);
```

在这个例子中，编译器可以对代码进行各种优化，例如：

- 将变量 `a` 和 `b` 的初始化与 `c` 的计算合并。
- 将常量折叠为 `printf("%d\n", 3);`。

只要最终输出的结果仍然是 `3`，编译器就遵循了“as-if-serial”原则。

**为什么需要as-if-serial原则？**

1. **保证正确性**：确保优化后的代码与源代码具有相同的行为，使程序员可以预测程序的行为。
2. **提高性能**：编译器可以自由地进行各种优化，提高程序的执行效率，而不需要担心改变程序的语义。

**Java中的"as-if-serial”原则**

在 Java 中，“as-if-serial”原则同样适用。Java 编译器和 Java 虚拟机（JVM）在进行优化时，也必须遵循这一原则。例如，JVM 可以对字节码进行即时编译（JIT）优化，但这些优化不能改变程序的可观察行为。

## volatile关键字实现原理？

在 Java 中，`volatile` 关键字确保对变量的读写操作是直接从主内存中进行的，而不是从线程的工作内存（缓存）中进行。这意味着一个线程对 `volatile` 变量的写操作对其他线程立即可见。

**实现原理**

1. **内存可见性**：
   - 当一个变量被声明为 `volatile` 时，Java 内存模型（JMM）确保所有线程在读取这个变量时，都能看到最新的写入。
   - 这通过在读写操作时插入内存屏障（Memory Barrier）实现。内存屏障是一种 CPU 指令，用于防止特定类型的内存操作重排序。
2. **禁止重排序**：
   - 编译器和 CPU 都会对代码进行优化，包括指令重排序。`volatile` 变量的读写操作不能与其他内存操作重排序，确保了操作的顺序性。
3. **轻量级同步**：
   - `volatile` 提供了一种比 `synchronized` 更轻量级的同步机制。虽然它不能保证原子性（例如，`volatile` 不能保证复合操作如 `i++` 的原子性），但它能确保内存可见性。

**代码示例**

```java
public class VolatileExample {
    private volatile boolean flag = false;

    public void writer() {
        flag = true;
    }

    public void reader() {
        if (flag) {
            // Do something
        }
    }
}
```

在这个例子中，`flag` 被声明为 `volatile`，因此当一个线程调用 `writer()` 方法时，另一个线程调用 `reader()` 方法时能够立即看到 `flag` 的变化。

# 锁

## 悲观锁和乐观锁？

悲观锁（Pessimistic Lock）和乐观锁（Optimistic Lock）是两种常见的并发控制机制，用于解决多线程环境下的数据一致性问题。它们的主要区别在于对待并发冲突的态度和处理方式。

**悲观锁**

**定义**：悲观锁是一种假设最坏情况的并发控制机制，认为每次操作都会发生并发冲突。因此，在操作数据之前，会先锁住数据，以确保其他线程无法同时访问该数据。

**特点**：

1. **锁定资源**：操作数据前，先获取锁，锁定资源，其他线程在锁释放之前无法访问该资源。
2. **开销较大**：频繁的锁定和解锁操作会增加系统开销，可能导致性能下降。
3. **适用于高冲突场景**：在高并发、高冲突的场景下，悲观锁可以有效避免数据不一致的问题。

**示例**：
在Java中，使用`synchronized`关键字或`ReentrantLock`类实现悲观锁。

```java
public class PessimisticLockExample {
    private final Object lock = new Object();

    public void criticalSection() {
        synchronized (lock) {
            // 临界区代码
            System.out.println("Thread " + Thread.currentThread().getName() + " is executing critical section.");
            try {
                Thread.sleep(1000); // 模拟处理时间
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    public static void main(String[] args) {
        PessimisticLockExample example = new PessimisticLockExample();

        Runnable task = () -> {
            for (int i = 0; i < 3; i++) {
                example.criticalSection();
            }
        };

        Thread thread1 = new Thread(task, "Thread-1");
        Thread thread2 = new Thread(task, "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```

**乐观锁**

**定义**：乐观锁是一种假设最佳情况的并发控制机制，认为并发冲突很少发生。因此，在操作数据时不加锁，而是在提交更新时检查数据是否发生变化，如果发生变化，则重试操作。

**特点**：

1. **不锁定资源**：操作数据时不加锁，而是在提交时验证数据的一致性。
2. **开销较小**：由于不需要频繁加锁和解锁，系统开销较小，性能较好。
3. **适用于低冲突场景**：在低并发、低冲突的场景下，乐观锁可以提高系统性能。

**实现方式**：

- **版本号机制**：每次更新数据时，增加版本号，通过比较版本号判断数据是否发生变化。
- **CAS（Compare And Swap）操作**：通过硬件指令实现的原子操作，比较并交换数据。

**示例**：
在Java中，可以使用`java.util.concurrent.atomic`包中的原子类实现乐观锁。

```java
import java.util.concurrent.atomic.AtomicInteger;

public class OptimisticLockExample {
    private final AtomicInteger version = new AtomicInteger(0);

    public void updateResource() {
        int currentVersion;
        do {
            currentVersion = version.get();
            // 模拟数据更新操作
            System.out.println("Thread " + Thread.currentThread().getName() + " is updating resource.");
            try {
                Thread.sleep(100); // 模拟处理时间
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        } while (!version.compareAndSet(currentVersion, currentVersion + 1));
    }

    public static void main(String[] args) {
        OptimisticLockExample example = new OptimisticLockExample();

        Runnable task = () -> {
            for (int i = 0; i < 3; i++) {
                example.updateResource();
            }
        };

        Thread thread1 = new Thread(task, "Thread-1");
        Thread thread2 = new Thread(task, "Thread-2");

        thread1.start();
        thread2.start();
    }
}
```

**总结**

- **悲观锁**：适用于高冲突场景，通过锁定资源保证数据一致性，但可能导致性能下降。
- **乐观锁**：适用于低冲突场景，通过版本号或CAS操作保证数据一致性，性能较好。

选择哪种锁机制取决于具体的应用场景和并发冲突的频率。在高并发、高冲突的情况下，悲观锁可能更合适；而在低并发、低冲突的情况下，乐观锁则能提供更好的性能。

## synchronized 关键字？

在 Java 中，`synchronized` 关键字用于实现线程同步，确保在同一时刻只有一个线程可以执行某个代码块或方法。这对于防止多个线程同时访问共享资源（如对象的字段或静态变量）而导致的不一致性问题非常重要。

1. **修饰实例方法**

   当 `sychronized` 修饰实例方法时，表示整个方法是同步的，只有一个线程可以访问这个方法，其他线程必须等待。

   ```java
   public class Example {
       public synchronized void synchronizedMethod() {
           // 代码块
       }
   }
   ```

2. **修饰静态方法**

   当 `sychronized` 修饰静态方法时，表示该方法是同步的，且是针对该类的所有实例。也就是说，所有线程在访问这个静态方法时都必须获得该类的锁。

   ```java
   public class Example {
       public static synchronized void synchronizedStaticMethod() {
           // 代码块
       }
   }
   ```

3. **修饰代码块**

   `sychronized` 还可以用来修饰代码块，指定某个对象作为锁。这样可以更细粒度地控制同步范围，提高性能。

   ```java
   public class Example {
       private final Object lock = new Object();
       
       public void synchronizedBlock() {
           synchronized (lock) {
               // 代码块
           }
       }
   }
   ```

## synchronized 实现原理？

`sychronized` 关键字在 Java 中用于实现线程同步，其底层实现依赖于对象的监视器锁（Monitor Lock）。在 Java 虚拟机（JVM）层面，`sychronized` 关键字的实现涉及到一些复杂的机制，如对象头、Monitor、和锁优化技术（如偏向锁、轻量级锁和重量级锁）。下面详细解释这些概念和其实现原理。

1. **对象头（Object Header）**

   在 Java 中，每个对象在内存中都有一个对象头（Object Header），其中包含了用于同步的锁信息。对象头的具体结构依赖于 JVM 的实现，但通常包括以下内容：

   - **Mark Word**：用于存储对象的哈希码、GC 分代年龄、锁状态标志等信息。
   - **Class Metadata Address**：指向对象的类元数据的指针。

2. **Monitor**

   Monitor 是 JVM 中用于实现同步的基本构件。每个对象都有一个与之关联的 Monitor，当线程进入同步块或同步方法时，需要获得该 Monitor 的所有权。Monitor 包含以下几个关键部分：

   - **Owner**：当前持有锁的线程。
   - **Entry List**：等待获取 Monitor 锁的线程列表。
   - **Wait Set**：调用 `Object.wait()` 方法后进入等待状态的线程列表。

3. **锁的状态**

   `sychronized` 的实现涉及多种锁状态，锁的状态会随着竞争情况的不同在几种状态之间转换：

   - **偏向锁（Biased Locking）**：当一个线程多次获得同一个锁时，锁会进入偏向模式，减少获取锁的开销。
   - **轻量级锁（Lightweight Locking）**：当偏向锁被另一个线程竞争时，锁会膨胀为轻量级锁，使用 CAS 操作进行竞争。
   - **重量级锁（Heavyweight Locking）**：当轻量级锁竞争失败时，锁会膨胀为重量级锁，线程会进入阻塞状态。

4. **锁的实现**

   - **偏向锁**

     偏向锁的设计目的是在无竞争的情况下减少锁的开销。当一个线程第一次获得锁时，锁进入偏向模式，Mark Word 中记录持有锁的线程 ID。如果同一个线程再次进入锁定块，只需简单地检查 Mark Word 中的线程 ID 是否与当前线程匹配即可。

   - **轻量级锁**

     当偏向锁被另一个线程竞争时，锁会膨胀为轻量级锁。轻量级锁使用 CAS 操作来竞争锁，失败的线程会自旋等待锁的释放。

   - **重量级锁**

     当轻量级锁竞争失败时，锁会膨胀为重量级锁，线程会进入阻塞状态。重量级锁使用操作系统的互斥量（Mutex）来实现，竞争失败的线程会被挂起，直到锁被释放。

5. **内存语义**

   `sychronized` 还提供了内存可见性保证：

   - **进入同步块**：线程会清空工作内存中的变量值，从主内存中重新读取。
   - **退出同步块**：线程会将工作内存中的变量值刷新到主内存。

6. **示例代码**

   ```java
   public class Counter {
       private int count = 0;
   
       public synchronized void increment() {
           count++;
       }
   
       public synchronized int getCount() {
           return count;
       }
   }
   ```

   在这个例子中，`increment` 和 `getCount` 方法都是同步方法，确保在同一时刻只有一个线程可以执行这些方法。

**总结**

`sychronized` 关键字在 Java 中提供了一种简单而有效的线程同步机制。其底层实现依赖于对象的监视器锁（Monitor Lock），并通过多种锁优化技术（如偏向锁、轻量级锁和重量级锁）来提高性能。理解 `sychronized` 的实现原理，对于编写高效、安全的多线程程序至关重要。

## 什么是ReentrantLock？

`ReentrantLock` 是 Java 中 `java.util.concurrent.locks` 包下的一个可重入锁（Reentrant Lock）实现。它提供了比 `synchronized` 关键字更灵活的锁机制。`ReentrantLock` 允许显式地获取和释放锁，并提供了更丰富的功能，如公平锁、公平锁、条件变量等。

**主要特点**

1. **可重入性**：与 `synchronized` 一样，`ReentrantLock` 是可重入的，这意味着同一个线程可以多次获取同一个锁，而不会发生死锁。
2. **公平锁和非公平锁**：`ReentrantLock` 可以配置为公平锁或非公平锁。公平锁按线程请求锁的顺序分配锁，而非公平锁则可能会使某些线程长时间等待锁。
3. **显式锁操作**：与 `synchronized` 关键字隐式地获取和释放锁不同，`ReentrantLock` 需要显式地调用 `lock()` 和 `unlock()` 方法来获取和释放锁。
4. **条件变量**：`ReentrantLock` 提供了条件变量（Condition），可以用来实现更复杂的线程间同步。

**使用示例**

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Counter {
    private int count = 0;
    private final Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        Counter counter = new Counter();

        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Final count: " + counter.getCount());
    }
}
```

**公平锁和非公平锁**

默认情况下，`ReentrantLock` 是非公平的。可以通过构造函数来创建公平锁，公平锁就是说会按照队列顺序获取和释放锁：

```java
Lock fairLock = new ReentrantLock(true); // 创建公平锁
Lock unfairLock = new ReentrantLock();   // 创建非公平锁（默认）
```

**条件变量**

`ReentrantLock` 提供了 `newCondition()` 方法来创建条件变量。条件变量可以用来实现更复杂的线程间同步，如生产者-消费者问题。

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BoundedBuffer {
    private final Lock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();
    private final Object[] items = new Object[100];
    private int putptr, takeptr, count;

    public void put(Object x) throws InterruptedException {
        lock.lock();
        try {
            while (count == items.length) {
                notFull.await();
            }
            items[putptr] = x;
            if (++putptr == items.length) putptr = 0;
            ++count;
            notEmpty.signal();
        } finally {
            lock.unlock();
        }
    }

    public Object take() throws InterruptedException {
        lock.lock();
        try {
            while (count == 0) {
                notEmpty.await();
            }
            Object x = items[takeptr];
            if (++takeptr == items.length) takeptr = 0;
            --count;
            notFull.signal();
            return x;
        } finally {
            lock.unlock();
        }
    }
}
```

**总结**

`ReentrantLock` 提供了一种比 `synchronized` 更灵活和强大的锁机制，适用于需要显式锁操作、条件变量和公平锁等高级功能的场景。在选择使用 `ReentrantLock` 还是 `synchronized` 时，应该根据具体需求和代码复杂度来决定。

## 什么是AQS？

AQS，全称是 **AbstractQueuedSynchronizer**，是Java并发包（`java.util.concurrent`）中的一个核心组件。AQS提供了一个框架，用于构建锁和同步器（如信号量、事件、读写锁等）。它通过一个先进先出（FIFO）的等待队列来管理线程的等待和唤醒，实现了线程的阻塞和唤醒机制。

AQS 的思想是，如果被请求的共享资源空闲，则当前线程能够成功获取资源；否则，它将进入一个等待队列，当有其他线程释放资源时，系统会挑选等待队列中的一个线程，赋予其资源。

**AQS的主要特点**

1. **FIFO等待队列**：AQS内部维护了一个FIFO等待队列，用于管理被阻塞的线程。每个节点代表一个等待线程，节点之间通过双向链表连接。
2. **状态**：AQS使用一个`volatile`的`int`类型变量来表示同步状态。子类通过继承AQS并实现其抽象方法来操作这个状态。
3. **独占模式和共享模式**：AQS支持两种模式：
   - **独占模式（Exclusive Mode）**：只有一个线程可以访问资源，如独占锁。
   - **共享模式（Shared Mode）**：多个线程可以共享访问资源，如信号量和读写锁。
4. **自定义同步器**：通过继承AQS并实现其抽象方法，可以轻松创建自定义的同步器。常见的同步器如ReentrantLock、CountDownLatch和Semaphore都是基于AQS实现的。

**AQS的工作原理**

1. **获取资源**：当一个线程尝试获取资源时，AQS会调用相应的`tryAcquire`或`tryAcquireShared`方法。如果资源不可用，线程会被加入等待队列并被阻塞。
2. **释放资源**：当一个线程释放资源时，AQS会调用`release`或`releaseShared`方法。这些方法会更新同步状态，并唤醒等待队列中的一个或多个线程。
3. **队列管理**：AQS内部维护的等待队列确保了线程以FIFO的顺序被唤醒，从而实现公平性。

## ReentrantLock实现原理？

`ReentrantLock` 的实现基于 `AbstractQueuedSynchronizer`（AQS）。

**内部结构**

`ReentrantLock` 内部主要依赖于一个静态内部类 `Sync`，它继承自 `AbstractQueuedSynchronizer`（AQS）。`Sync` 有两个子类，分别用于实现公平锁和非公平锁：

- `FairSync`：实现公平锁。
- `NonfairSync`：实现非公平锁。

**锁的获取**

- **非公平锁**

  非公平锁是 `ReentrantLock` 的默认实现。锁的获取过程如下：

  1. 尝试直接获取锁。如果状态为 0，则通过 CAS 操作将状态设置为 1，并将当前线程设置为持有锁的线程。
  2. 如果获取锁失败，则调用 `acquire` 方法进入 AQS 的等待队列，等待锁的释放。

  ```java
  static final class NonfairSync extends Sync {
      final void lock() {
          if (compareAndSetState(0, 1))
              setExclusiveOwnerThread(Thread.currentThread());
          else
              acquire(1);
      }
  }
  ```

- **公平锁**

  公平锁会按照请求的顺序获取锁。锁的获取过程如下：

  1. 尝试直接获取锁。如果状态为 0 且没有其他线程在等待队列中排队，则通过 CAS 操作将状态设置为 1，并将当前线程设置为持有锁的线程。
  2. 如果获取锁失败或者有其他线程在等待队列中排队，则调用 `acquire` 方法进入 AQS 的等待队列，等待锁的释放。

  ```java
  static final class FairSync extends Sync {
      final void lock() {
          acquire(1);
      }
  
      protected final boolean tryAcquire(int acquires) {
          final Thread current = Thread.currentThread();
          int c = getState();
          if (c == 0) {
              if (!hasQueuedPredecessors() && compareAndSetState(0, acquires)) {
                  setExclusiveOwnerThread(current);
                  return true;
              }
          } else if (current == getExclusiveOwnerThread()) {
              int nextc = c + acquires;
              if (nextc < 0) // overflow
                  throw new Error("Maximum lock count exceeded");
              setState(nextc);
              return true;
          }
          return false;
      }
  }
  ```

**锁的释放**

锁的释放过程如下：

1. 调用 `release` 方法。
2. 减少状态值（表示锁的重入次数）。
3. 如果状态值减为 0，则将持有锁的线程设置为 `null` 并唤醒等待队列中的下一个线程。

```java
protected final boolean tryRelease(int releases) {
    int c = getState() - releases;
    if (Thread.currentThread() != getExclusiveOwnerThread())
        throw new IllegalMonitorStateException();
    boolean free = false;
    if (c == 0) {
        free = true;
        setExclusiveOwnerThread(null);
    }
    setState(c);
    return free;
}
```

## 什么是CAS？

CAS，全称为 Compare-And-Swap（比较并交换），是一种用于实现并发算法的原子操作。CAS 操作是无锁编程的一种常见方式，用于在多线程环境下实现变量的安全更新。它的基本思想是：只有当变量的当前值与预期值相等时，才将其更新为新值，否则不进行更新。

**工作原理**

CAS 操作通常需要三个操作数：

1. **内存位置（V）**：需要被操作的变量的内存地址。
2. **预期值（A）**：期望变量当前持有的值。
3. **新值（B）**：准备更新为的新值。

CAS 操作的步骤如下：

1. 读取变量 V 的当前值。
2. 比较当前值与预期值 A 是否相等。
   - 如果相等，则将当前值更新为新值 B。
   - 如果不相等，则不进行更新，并返回当前值。

CAS 操作是原子的，意味着在执行过程中不会被其他线程打断。

**优点**

1. **高效**：CAS 操作通常在硬件级别实现，效率很高。
2. **无锁**：通过 CAS，可以避免使用传统的锁机制，从而减少锁竞争和上下文切换，提高并发性能。
3. **非阻塞**：CAS 操作不会像锁一样阻塞线程，线程可以不断重试，直到成功。

**缺点**

1. **ABA 问题**：如果一个变量在 CAS 操作过程中从值 A 变成 B，又变回 A，那么 CAS 操作无法检测到这个变化。解决 ABA 问题的一种常见方法是使用版本号，每次更新变量时同时更新版本号。
2. **自旋开销**：在高并发环境下，如果很多线程同时尝试更新同一个变量，可能会导致大量的 CAS 操作失败，线程不断重试，造成较大的 CPU 开销。
3. **只能操作单个变量**：CAS 操作通常只能用于单个变量的更新，无法直接用于多个变量的原子操作。

**Java中的CAS**

在 Java 中，CAS 操作主要通过 `java.util.concurrent.atomic` 包中的类来实现，如 `AtomicInteger`、`AtomicLong`、`AtomicReference` 等。这些类内部使用了 `Unsafe` 类的 CAS 方法来实现原子操作。

```java
import java.util.concurrent.atomic.AtomicInteger;

public class CASExample {
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(0);

        // 执行 CAS 操作
        boolean success = atomicInteger.compareAndSet(0, 1);

        System.out.println("CAS 操作成功: " + success);
        System.out.println("当前值: " + atomicInteger.get());
    }
}
```

在上述示例中，`compareAndSet` 方法执行了 CAS 操作，将 `atomicInteger` 的值从 0 更新为 1。

## 原子操作类？

Java 的原子操作类主要集中在 `java.util.concurrent.atomic` 包中，这些类提供了一些基本数据类型的原子操作，确保在并发环境下进行线程安全的操作。下面是一些主要的原子操作类及其作用：

1. **AtomicInteger**

   用于对 `int` 类型变量进行原子操作。

   ```java
   import java.util.concurrent.atomic.AtomicInteger;
   
   public class AtomicIntegerExample {
       public static void main(String[] args) {
           AtomicInteger atomicInteger = new AtomicInteger(0);
   
           // 获取当前值
           int currentValue = atomicInteger.get();
           System.out.println("Current value: " + currentValue);
   
           // 设置新值
           atomicInteger.set(5);
           System.out.println("New value: " + atomicInteger.get());
   
           // 通过 CAS 操作设置新值
           boolean success = atomicInteger.compareAndSet(5, 10);
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicInteger.get());
   
           // 自增操作
           int incrementedValue = atomicInteger.incrementAndGet();
           System.out.println("Incremented value: " + incrementedValue);
       }
   }
   ```

2. **AtomicLong**

   用于对 `long` 类型变量进行原子操作。

   ```java
   import java.util.concurrent.atomic.AtomicLong;
   
   public class AtomicLongExample {
       public static void main(String[] args) {
           AtomicLong atomicLong = new AtomicLong(0L);
   
           // 获取当前值
           long currentValue = atomicLong.get();
           System.out.println("Current value: " + currentValue);
   
           // 设置新值
           atomicLong.set(5L);
           System.out.println("New value: " + atomicLong.get());
   
           // 通过 CAS 操作设置新值
           boolean success = atomicLong.compareAndSet(5L, 10L);
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicLong.get());
   
           // 自增操作
           long incrementedValue = atomicLong.incrementAndGet();
           System.out.println("Incremented value: " + incrementedValue);
       }
   }
   ```

3. **AtomicBoolean**

   用于对 `boolean` 类型变量进行原子操作。

   ```java
   import java.util.concurrent.atomic.AtomicBoolean;
   
   public class AtomicBooleanExample {
       public static void main(String[] args) {
           AtomicBoolean atomicBoolean = new AtomicBoolean(false);
   
           // 获取当前值
           boolean currentValue = atomicBoolean.get();
           System.out.println("Current value: " + currentValue);
   
           // 设置新值
           atomicBoolean.set(true);
           System.out.println("New value: " + atomicBoolean.get());
   
           // 通过 CAS 操作设置新值
           boolean success = atomicBoolean.compareAndSet(true, false);
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicBoolean.get());
       }
   }
   ```

4. **AtomicReference**

   用于对引用类型变量进行原子操作。

   ```java
   import java.util.concurrent.atomic.AtomicReference;
   
   public class AtomicReferenceExample {
       public static void main(String[] args) {
           AtomicReference<String> atomicReference = new AtomicReference<>("initial");
   
           // 获取当前值
           String currentValue = atomicReference.get();
           System.out.println("Current value: " + currentValue);
   
           // 设置新值
           atomicReference.set("updated");
           System.out.println("New value: " + atomicReference.get());
   
           // 通过 CAS 操作设置新值
           boolean success = atomicReference.compareAndSet("updated", "final");
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicReference.get());
       }
   }
   ```

5. **AtomicStampedReference**

   用于解决 CAS 操作中的 ABA 问题，通过引入版本号（戳）来确保变量的正确更新。

   ```java
   import java.util.concurrent.atomic.AtomicStampedReference;
   
   public class AtomicStampedReferenceExample {
       public static void main(String[] args) {
           String initialRef = "initial";
           int initialStamp = 0;
           AtomicStampedReference<String> atomicStampedReference = new AtomicStampedReference<>(initialRef, initialStamp);
   
           // 获取当前值和版本号
           int[] stampHolder = new int[1];
           String currentValue = atomicStampedReference.get(stampHolder);
           int currentStamp = stampHolder[0];
           System.out.println("Current value: " + currentValue + ", Current stamp: " + currentStamp);
   
           // 设置新值和版本号
           atomicStampedReference.set("updated", currentStamp + 1);
           System.out.println("New value: " + atomicStampedReference.get(stampHolder) + ", New stamp: " + stampHolder[0]);
   
           // 通过 CAS 操作设置新值和版本号
           boolean success = atomicStampedReference.compareAndSet("updated", "final", currentStamp + 1, currentStamp + 2);
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicStampedReference.get(stampHolder) + ", Updated stamp: " + stampHolder[0]);
       }
   }
   ```

6. **AtomicMarkableReference**

   类似于 `AtomicStampedReference`，但使用一个布尔标记来解决 ABA 问题。

   ```java
   import java.util.concurrent.atomic.AtomicMarkableReference;
   
   public class AtomicMarkableReferenceExample {
       public static void main(String[] args) {
           String initialRef = "initial";
           boolean initialMark = false;
           AtomicMarkableReference<String> atomicMarkableReference = new AtomicMarkableReference<>(initialRef, initialMark);
   
           // 获取当前值和标记
           boolean[] markHolder = new boolean[1];
           String currentValue = atomicMarkableReference.get(markHolder);
           boolean currentMark = markHolder[0];
           System.out.println("Current value: " + currentValue + ", Current mark: " + currentMark);
   
           // 设置新值和标记
           atomicMarkableReference.set("updated", !currentMark);
           System.out.println("New value: " + atomicMarkableReference.get(markHolder) + ", New mark: " + markHolder[0]);
   
           // 通过 CAS 操作设置新值和标记
           boolean success = atomicMarkableReference.compareAndSet("updated", "final", !currentMark, currentMark);
           System.out.println("CAS operation successful: " + success);
           System.out.println("Updated value: " + atomicMarkableReference.get(markHolder) + ", Updated mark: " + markHolder[0]);
       }
   }
   ```

**总结**

Java 的原子操作类提供了一种高效的方式来在并发环境下进行线程安全的操作。这些类通过底层的 CAS 操作来确保原子性，避免了传统锁机制带来的性能开销。它们在实现无锁编程和提高并发性能方面起到了重要作用。

## 线程死锁？

在Java编程中，死锁是一种常见的并发问题，它发生在两个或多个线程彼此等待对方释放锁，从而导致所有线程都无法继续执行。理解和避免死锁对于编写高效且可靠的并发程序至关重要。

**什么是死锁？**

死锁是指两个或多个线程在等待彼此持有的资源时形成的一种循环等待状态，导致所有线程都无法继续执行。死锁的四个必要条件是：

1. **互斥条件**：一个资源每次只能被一个线程使用。
2. **占有且等待条件**：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
3. **不剥夺条件**：线程已获得的资源在未使用完之前不能被剥夺，只能在使用完时由线程自己释放。
4. **循环等待条件**：若干线程之间形成一种头尾相接的循环等待资源关系。

**死锁的示例？**

```java
public class DeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock 1...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 1: Waiting for lock 2...");
                synchronized (lock2) {
                    System.out.println("Thread 1: Holding lock 1 and lock 2...");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock 2...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 2: Waiting for lock 1...");
                synchronized (lock1) {
                    System.out.println("Thread 2: Holding lock 2 and lock 1...");
                }
            }
        });

        thread1.start();
        thread2.start();
    }
}
```

在这个例子中，`thread1` 先获取 `lock1`，然后等待 `lock2`；而 `thread2` 先获取 `lock2`，然后等待 `lock1`。这就形成了一个循环等待条件，导致死锁。

**如何检测死锁？**

Java 提供了一些工具和方法来检测死锁：

1. **线程转储（Thread Dump）**：使用 `jstack` 工具可以生成 Java 进程的线程转储信息，分析其中的线程状态以检测死锁。

   ```sh
   jstack <pid>
   ```

2. **Java 虚拟机（JVM）监控工具**：例如 JConsole、VisualVM 等，它们可以监控 JVM 并检测死锁。

**如何避免死锁？**

避免死锁的方法包括：

1. **避免嵌套锁**：尽量减少嵌套锁的使用，降低发生死锁的可能性。
2. **锁的顺序**：确保所有线程按照相同的顺序获取锁，以避免循环等待。
3. **使用超时**：使用带超时的锁机制，例如 `tryLock`，避免长时间等待。
4. **死锁检测**：在高并发环境中，定期检测并处理死锁。

**示例：避免死锁**

```java
public class AvoidDeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            acquireLocks(lock1, lock2);
            try {
                System.out.println("Thread 1: Holding lock 1 and lock 2...");
            } finally {
                releaseLocks(lock1, lock2);
            }
        });

        Thread thread2 = new Thread(() -> {
            acquireLocks(lock1, lock2);
            try {
                System.out.println("Thread 2: Holding lock 1 and lock 2...");
            } finally {
                releaseLocks(lock1, lock2);
            }
        });

        thread1.start();
        thread2.start();
    }

    private static void acquireLocks(Object lock1, Object lock2) {
        while (true) {
            synchronized (lock1) {
                if (tryLock(lock2)) {
                    return;
                }
            }
        }
    }

    private static boolean tryLock(Object lock) {
        try {
            synchronized (lock) {
                return true;
            }
        } catch (Exception e) {
            return false;
        }
    }

    private static void releaseLocks(Object lock1, Object lock2) {
        synchronized (lock1) {
            synchronized (lock2) {
                // release locks
            }
        }
    }
}
```

在这个示例中，线程按照相同的顺序获取锁，从而避免了循环等待条件，减少了死锁的可能性。



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

