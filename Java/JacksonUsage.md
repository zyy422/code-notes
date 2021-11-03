# Jackson 简单使用
- 序列化
  - 将对象转换为json或者xml格式或其他格式的报文
- 反序列化
  - 将json或者其他格式的报文转换为Java对象
  
在Springmvc框架中，默认使用的是Jackson来对请求数据和相应报文分别进行反序列化和序列化，带有```@ResponseBody```注解的请求的返回对象，都会被序列化为json报文格式。

## 简单实用jackson
### 依赖导入
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson.version}</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>${jackson.version}</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>${jackson.version}</version>
</dependency>
```
在本文的案例中，第一个依赖是必备依赖，其他的依赖可以暂时不用。

```java
// 反序列化
ObjectMapper objectMapper = new ObjectMapper();
String carJson ="{ \"brand\" : \"Mercedes\", \"doors\" : 5 }";
Car car = objectMapper.readValue(carJson, Car.class);
System.out.println(car.toString());

// 序列化
Car car2 = new Car();
car2.setBrand("奥迪");
car2.setDoors(4);
String jsonCar = objectMapper.writeValueAsString(car2);
System.out.println(jsonCar);
```
实用ObjectMapper对象的readValue进行反序列化对象，而writeValue方法可以序列化，可以输出到不同的目标中。

以下为输出结果
```
jackson.Car@4d41cee
{"brand":"奥迪","doors":4}
```

## 总结
Jackson仅仅是一种用于序列化的工具，与其具有相同功能的有fast json等。该工具知晓即可，并不需要去实现。