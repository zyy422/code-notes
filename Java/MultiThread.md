# Java 多线程
- 继承Thread类
- 实现Runnable接口
- 实现Callable接口

## 继承Thread类
创建一个线程类并继承Thread，实现线程的run方法
```java
        // 继承Thread实现
        Thread thread1 = new Thread() {
            @Override
            public void run() {
                ToolBox.log("线程1被执行了");
            }
        };
        thread1.start();
```

## 实现Runnable接口
```java
        // 实现Runnable接口实现
        Thread thread2 = new Thread(()->{
            ToolBox.log("线程2被执行了");
        });
        thread2.start();
```
Runnable接口封装的异步任务，可以将其认为是一个没有参数和返回值的任务。

## 实现callable接口
```java
        // 实现Callable接口实现
        FutureTask<String> task3 = new FutureTask(()->{
            ToolBox.log("线程3被执行了");
            return "Thread3 End";
        });
        Thread thread3 = new Thread(task3);
        thread3.start();
        ToolBox.log("主线程获取的线程3返回信息:" + task3.get());
```
Callable任务与Runnable的方式相似，但是其有返回值。注意```task3.get()```将阻塞线程,直到线程返回。如果超时，则将抛出一个TimeoutException。

FutureTask这个类继承自Future有几个函数，可以初步掌握一下.

- cancel(boolean): Attempts to cancel execution of this task.
- isCancelled(): Returns true if this task was cancelled before it completed normally.
- isDone(): Returns true if this task was cancelled before it completed normally.
- get(): nothing to say
