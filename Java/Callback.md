# Java回调函数

## 定义
回调是一种双向的模式，例如A调用B方法，B在执行完成之后又调用A方法。


在C和C++中，**回调函数**是一个通过函数指针调用的函数。如果你把函数的地址当作参数传递给**另一个函数**时，**另一个函数**来调用这个指针地址所指向的函数，这就叫**回调函数**

## 案例和场景
```java
public static void main(String[] args) {
    new TestJob().doTest(()->{
        System.out.println("这里的代码等着你来回调我");
    });
}
```
```java
public class TestJob {
    public void doTest(Callaback callaback) {
        for (int i = 0; i < 10; i++) {
            System.out.println("do something...");
        }

        // 核心代码，回调
        callaback.execute();

        for (int i = 0; i < 10; i++) {
            System.out.println("do something...");
        }
    }
}
```
```java
public interface Callaback {

    void execute();
}
```

## 总结
从上面案例场景可以看到，lambda表达式、匿名内部类和回调函数，这三者实际上是本质相同，不同的角度面而已。
- 对于匿名内部类来说
    - java 8之前，不支持lambda表达式，只能传一个对象过去。有的类只需要构建一次，后面不会使用。因此创建一个匿名内部类，构建一个没有名字的对象传过去。
- 对于回调函数
  -  在C和C++中，函数是可以通过指针进行直接调用的。因此，直接将一个指针传递过去，让调用的函数在需要时，通过指针来调用即可。
- 对于lambda表达式
  - lambda属于一种简写，本身属于匿名内部类的另一种形势。由于lambda表达式是一个方法的实现，因此形参必须是只有一个方法的接口 