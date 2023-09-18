
Constant被ConstantPool集中管理。

下面是ConstantPool的定义：
```java
public abstract class ConstantPool<T extends Constant<T>>
```

# 属性

下面是ConstantPool中的属性。
```java
private final ConcurrentMap<String, T> constants = PlatformDependent.newConcurrentHashMap();  
  
private final AtomicInteger nextId = new AtomicInteger(1);
```

