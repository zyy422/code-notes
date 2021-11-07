# Observer Pattern （观察者模式）

## 基本概念
观察者模式是非常重要的设计模式，譬如当下热火朝天的Zookeeper，便是基于观察者模式所设计出来的。该设计模式应该主要包括两个部分：
- **观察者们**： 当一个事件发生，所有的观察者们都会收到通知，或者说他们的认知都会被更新。就像继承者们，每个人都有资格继承财产，哈哈；
- **目标对象**： 这个目标对象就是那个发生的事件，这个事件的触发与否会让所有观察者们聚精会神地盯着。就像一笔巨大的财产，继承者们都在等着继承呢。

## 实现方法
首先，我们应该创建一个被观察的**目标对象**。Enum...这个目标对象需要有哪些元素信息呢？
- First of All, 这个受到观察的对象应该有个自己的属性，这个属性的变化应该应该引起所有观察者们的重视。
- Secondly, 它应该有一大堆的观察者们，这些观察者们应该是个List。某个属性的变化，应该对这个List的所有观察者们进行逐一通知。
- Third, 观察者们应该允许被动态增减。万一有哪个新的私生子出现，想来观察这笔财产，那我们应该把它加上，
- Finally, 这批观察者们应该有个公共的接口(规范)，想要被通知到，就需要实现这个规范(接口)。

因此，目标对象可以这么设计：
```java
/**
 * 目标对象，也就是被观察的对象
 */
public class Subject {
    private List<Observer> observers = new LinkedList<>();
    
    // 相当于财产
    private int state;

    public int getState() {
        return state;
    }

    /**
     * 状态修改，观察者们都将收到通知
     * @param state 状态
     */
    public void setState(int state) {
        this.state = state;
        notifyAllObservers();
    }

    /**
     * 增加一个观察者
     * @param observer 被添加的观察者
     */
    public void attach(Observer observer) {
        observers.add(observer);
    }

    private void notifyAllObservers() {
        observers.forEach(observer ->{observer.update();});
    }
}
```

对于观察者们，其规范接口和实现可以如下:
```java
/**
 * 观察者们的抽象接口
 */
public abstract class Observer {
    public abstract void update();
}

/**
 * 一个继承者
 */
public class HexaObserver extends Observer{

    public HexaObserver(Subject subject) {
        subject.attach(this);
    }

    @Override
    public void update() {
        System.out.println("十六进制观察者观察到了这一改变，更新完毕！");
    }
}

/**
 * 另一个继承者
 */
public class BinaryObserver extends Observer{
    /**
     * 构建观察者时，需要提供一个观察对象
     * @param subject 被观察对象
     */
    public BinaryObserver(Subject subject) {
        subject.attach(this);
    }

    @Override
    public void update() {
        System.out.println("二进制观察者观察到了这一改变！");
    }
}
```
最后，主函数的调用灰常简单。
```java
public static void main(String[] args) {
    Subject subject = new Subject();
    BinaryObserver binaryObserver = new BinaryObserver(subject);
    HexaObserver hexaObserver = new HexaObserver(subject);
    subject.setState(1);
}
```

## 总结
为了简便性，以上便是观察者模式的简单实现。其实，有个问题就是，每个观察者没有办法知道这个```state```的变换值具体是多少。因为，我们在构建这个观察者对象的时候，没有将目标对象的属性进行保存。解决办法是在进行构造观察者对象时，每个观察者对象再存一份目标对象的引用。这样，目标对象的每次属性变化，观察者们都可以直接通过引用来获取其值了。```update()```方法不需要参数，因为仅仅需要触发这一变化就好了。综上便是观察者模式的理解和实现，若有不足，望批评指正～