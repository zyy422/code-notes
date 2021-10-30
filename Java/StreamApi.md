# Java Stream API

在Java 8 中，推出的一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。

流Stream大大简化了对Java集合类的操作，让曾今用for循环的方式变得更加优雅。

---
很喜欢runbood.com上所画的一个图，非常形象和生动描绘了这个Stream API的作用。
```
+--------------------+       +------+   +------+   +---+   +-------+
| stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
+--------------------+       +------+   +------+   +---+   +-------+
```
以上的流程可以这么描述：
- 先获取一个stream流对象
- 对stream流对象进行过滤操作
- 对Stream流对象进行排序操作
- 对stream流对象进行映射操作
- 一通操作之后，对最终的流进行结果获取收集

## stream流的来源可以是哪些？
- 集合
  - Collection.stream()方法获取集合的流对象，该流为串行流。
- 数组
  - ```Arrays.stream(T[] array)```方法获取流对象
- I/O Channel
- ```parallelStream()``` 为集合创建并行流
- 等等
  

## example
如下示例为对字符串数组进行截取第一位（map映射），并且该位的ASC大于等于‘c’后，再进行排序，仅仅保留前两个元素
```java
        // 流的方式操作数组
        List<String> strings = Arrays.asList("lily","bob","tom","cat","mom");

        List<String> res = strings.stream()
                .map(e->e.substring(0,1))
                .filter(e->e.charAt(0) >= 'c')
                .sorted()
                .limit(2)
                .collect(Collectors.toList());

        res.forEach(s -> {System.out.println("截取第一位字母且大于c的有： " + s);});
```

## 总结
通过使用流，可以很方便地对集合等元素进行操作。我们仅仅需要写出要做什么，并不需要写具体是怎么实现的，同时配合上lambda表达式，使得原本冗余和复杂的源代码变得更加优雅，以简为美。