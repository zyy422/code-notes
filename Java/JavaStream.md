# 深入理解Java流库
流库提供了一种比集合更高的概念级别上指定计算的数据视图。

## 流和集合的差异性
- 流不存储元素，这些元素可能按需生成，可能存粗在底层的集合中
- 流的操作不会修改其数据源
- 操作是尽可能惰性执行的

## 流的创建
```java
Stream<String> stream1 = Stream.of("Hello world", "Good body");
stream1.forEach(System.out::println);

String preparedStr = "Quora is a place to gain and share knowledge. It's a platform to ask questions and connect with people who contribute unique insights and quality answers.";
Stream<String> stream2 = Stream.of(preparedStr.split("\\."));
stream2.forEach(System.out::println);
```

