netty提供常量来帮助Channel类完成一系列配置。

Constant接口的定义如下：
```java
public interface Constant<T extends Constant<T>> extends Comparable<T> {
	int id();
	String name();
}
```

