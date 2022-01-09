# 枚举类实现单例模式

```java
public class Singleton {
    private Singleton(){}

    public void print(){
        System.out.print("Hello World");
    }

    private enum SingletonEnum{
        INSTANCE;
        Singleton singleton = null;
        SingletonEnum(){
            singleton = new Singleton();
        }
    }

    public static Singleton getInstance(){
        return SingletonEnum.INSTANCE.singleton;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        singleton.print();
    }
}
```