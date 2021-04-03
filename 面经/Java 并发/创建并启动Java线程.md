在Java程序中线程就像虚拟 CPU 一样执行 java 代码，当 java 程序从 main() 方法开始在主线程中执行，一个特殊的线程被 JVM 创建用来运行程序。在程序中可以创建并且开启多个线程来运行部分程序代码，开启的线程与主线程并行运行。

java 线程对象就像其他 java 对象一样。线程是类 java.lang.Thread 的实例，在对象基础上，java 线程也执行代码，在这个 Java 线程教程中，我将介绍如何创建并开启线程。

## 创建开启线程

创建一个 java 线程就像这样：

```java
Thread thread = new Thread();
```

开启 java 线程可以调用 start() 方法，像这样：

```java
thread.start();
```

这个例子线程不能执行指定任何代码，然而，线程启动后立刻停止。

这有两种执行线程执行代码的方式，第一种创建一个 Thread 的子类，并覆盖线程的 run() 方法，第二种是通过一个实现 Runnable(java.lang.Runnable) 的类来构造线程，下面是两种方式。

## 线程类（Thread Subclass）

第一种方式在线程中指定代码运行，创建标准 Thread 的子类并覆盖 run() 方法，线程执行 start() 方法后执行 run()。这是一个创建 Java Thread 子类：

```java
  public class MyThread extends Thread {

    public void run(){
       System.out.println("MyThread running");
    }
  }
```

创建并开始线程可以用下面这种方式：

```java
  MyThread myThread = new MyThread();
  myTread.start();
```

一调用 start() 方法，线程就开始，他将不会等待 run() 方法执行完。run() 方法将通过不同的 CPU 执行，当 run() 方法执行将打印文字“MyThread running”。

也可以创建 Thread 匿名子类：
```java
  Thread thread = new Thread(){
    public void run(){
      System.out.println("Thread Running");
    }
  }

  thread.start();
```

这个例子将会打印 “Thread Running” 一次， run() 方法执行通过创建线程。

## Runnable 接口实现（Runnable Interface Implementation）
第二种指定线程执行代码的方式，是创建一个类实现 java.lang.Runnable 接口。java 对象实现 Runnable 接口可以通过 java 线程执行。如何完成操作在文章最后介绍。

Runnable 接口是 java 平台标准的接口，Runnable 接口只有一个方法 run()，Runnable 接口主要可以看：

```java
public interface Runnable() {

    public void run();

}
```

任何线程支持，当他执行必须实现 run() 方法，下面是三种实现 Runnable 接口的方式：
1. 创建一个 java 类实现 Runnable 接口
2. 创建一个匿名类实现 Runnable 接口
3. 创建一个 java Lambda 实现 Runnable 接口

下面介绍这三种方式：

### java 类实现 Runnable
第一种方式实现 java Runnable 接口通过创建自己的 java 类实现 Runbable 接口，这有一个例子自定义 java 类实现 Runnable 接口：
```java
  public class MyRunnable implements Runnable {

    public void run(){
       System.out.println("MyRunnable running");
    }
  }
```

所有 Runnable 实现打印文字 “MyRunnable running”。打印文字后，运行 run() 方法退出，并且线程运行 run() 方法将停止。

### 匿名实现 Runnable

可以创建匿名实现 Runnable，这有一个实现 Runnable 接口的匿名 java 类：
```java
Runnable myRunnable =
    new Runnable(){
        public void run(){
            System.out.println("Runnable running");
        }
    }
```
除了是匿名类，此例子与自定义类实现 Runnable 接口相似。

### java Lambda 实现 Runnable

第三种方式实现 Runnable 接口通过 java Lambda 实现 Runnable 接口，因为 Runnable 接口仅仅只有一个不重要的方法，或者实际上是功能性 java 接口。

这有一个例子是 java Lambda 表示，实现 Runnable 接口：
```java
Runnable runnable =
        () -> { System.out.println("Lambda Runnable running"); };
```

### 开启一个 Runnable 线程

通过一个类的实例 run() 方法执行一个线程，匿名类或者 Lambda 表达式实现 Runnable 接口的实现。例如：

```java
Runnable runnable = new MyRunnable(); // or an anonymous class, or lambda...

Thread thread = new Thread(runnable);
thread.start();
```
当线程开始将调用 run() 方法在 MyRunnable 实例在执行自己的 run() 方法，下面是例子，打印 “MyRunnable running”

## 子类或者 Runnable

没有规则说那个方法更好，两种方法都可以。就我而言，我倾向于 Runnable ，实现一个线程接口示例是复杂的。当有 Runnable 的执行通过线程池，通过 Runnable 接口丢列实现直到线程通过池空闲，基于 Thread 子类就会更复杂。

有时候 必须实现 Runnable 而不是 Thread 子类，例如 如果创建一个线程子类可以执行多个 Runnable ，典型的就是 线程池的实现。

## 公共陷阱：调用 run() 而不是 start()

当创建并且开始一个线程是通常做法是调用 run() 方法而不是线程的 start()，例如：
```java
  Thread newThread = new Thread(MyRunnable());
  newThread.run();  //should be start();
```

起初可能不会意识到，因为 Runnable 的 run() 方法执行就像预期一样。然而仅仅被创建而未能通过线程执行，相反 run() 方法需要通过线程创建线程的方式执行。换而言之，上面线程执行两行代码，想要执行 MyRnnable 实例中的 run() 方法在心线程中，必须调用 newThread.start() 方法。

