# 你真的了解工厂模式吗？

## 1.一个例子

下面的代码片段是否发现有不妥之处呢？虽然我们日常都这么写，但是bar这个引用变量，和FooBar对象开辟的内存空间耦合了。是的，这个引用只能指向FooBar这个代码块。

```java
// 构建对象
Bar bar = new FooBar();
// 使用构建对象的方法
bar.doSomething();
```

## 2. 初步解决一些问题

以下这个应用程序可以根据执行Java程序的命令参数来创建对象并做一些事情了。

```java
public static void main(String[] args) {
    Bar bar = null;

    if ("foo".equals(args[0])) {
        bar = new FooBar();
    } 
    if ("bar".equals(args[0])) {
        bar = new Bar();
    } 
    
    if (bar != null) {
        bar.doSomething();
    }
}
```

## 3.不足之处

如果我们想要新添一个Bar的子类用于并且使用它的doSomething方法时，必须修改原始的代码了，这不符合开闭原则。