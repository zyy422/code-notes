# Java函数式编程

## 一些补充知识
在编程语言中，一般抽象程度越低，其执行的效率便会越高。如汇编，其最接近于机器的底层实现，因此其执行效率非常高。

而函数式编程，其抽象程度属于非常高的层次。

## 什么是函数式接口?
只有一个抽象的方法的接口叫做**函数式接口**(Functional Interface)，在JDK中大部分函数式接口都会标记上```@FunctionalInterface```注解，并不是所有的函数式接口都要写```@FunctionalInterface```注解，只是用来区分哪些是函数式接口的，如果标记了这个注解但又存在多个抽象方法，在编译时期便会报错。

我们先定义了一个接口，其仅存在一个test方法。
```java
public interface Predicate {
    String test(String inputStr);
}
```
对于一些只需要执行一次的代码，我们可以放弃先构建一个实现类再重写方法的模式，直接使用Lambda表达式进行代替，其代码如下。
```java
Predicate predicate = e->{
    System.out.println("Hello world");
    return "a";
};
predicate.test("a");
```
以上便是函数式编程了。

## 一些内置的函数式接口
内置函数式接口都置于```java.util.function```包中，主要分为以下四种
- Function
  - 一个入参，一个出参。其定义的借口里都是范型，因此必须为基础类，不可以为基本类型。
- Consumer
  - 一个入参，无出参。
- Supplier
  - 无入参，一个出参
- Predicate
  - 一个入参，出参类型为```boolean```

在Stream API的filter方法中，其针对输入，便规定了为Predicate类型的借口，因此我们必须写一个lambda表达式，针对入参进行判断，并返回boolean。
```java
Stream<T> filter(Predicate<? super T> predicate);
```

由于以上四个内置接口，入参都为范型，无法替代基本数据类型（int, long, double...）。因此，必须针对入参类型为这些基本数据类型定制特殊和单一的借口，其如下所示。
- IntBinaryOperator
- IntConsumer
- IntFunction
- IntPredicate
- IntSupplier
  - 以该类型为例，无入参，返回一个int类型的出参