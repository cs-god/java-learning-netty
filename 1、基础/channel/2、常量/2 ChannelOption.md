首先看ChannelOption的定义：
```java
public class ChannelOption<T> extends AbstractConstant<ChannelOption<T>>
```
可以看到，ChannelOption本质上是常量。

# 属性

ChannelOption抽象类中就只有一个属性，就是ConstantPool。
```java
private static final ConstantPool<ChannelOption<Object>> pool = new ConstantPool<ChannelOption<Object>>() {  
    @Override  
    protected ChannelOption<Object> newConstant(int id, String name) {  
        return new ChannelOption<Object>(id, name);  
    }  
}
```


# 预定义常量（重点）

ChannelOption提供了一系列定义好的ChannelOption常量。
```java
public static final ChannelOption<ByteBufAllocator> ALLOCATOR = valueOf("ALLOCATOR");  
public static final ChannelOption<RecvByteBufAllocator> RCVBUF_ALLOCATOR = valueOf("RCVBUF_ALLOCATOR");  
public static final ChannelOption<MessageSizeEstimator> MESSAGE_SIZE_ESTIMATOR = valueOf("MESSAGE_SIZE_ESTIMATOR");  
  
public static final ChannelOption<Integer> CONNECT_TIMEOUT_MILLIS = valueOf("CONNECT_TIMEOUT_MILLIS");  
public static final ChannelOption<Integer> MAX_MESSAGES_PER_WRITE = valueOf("MAX_MESSAGES_PER_WRITE");  
public static final ChannelOption<Integer> WRITE_SPIN_COUNT = valueOf("WRITE_SPIN_COUNT");  

public static final ChannelOption<WriteBufferWaterMark> WRITE_BUFFER_WATER_MARK =  
        valueOf("WRITE_BUFFER_WATER_MARK");  
  
public static final ChannelOption<Boolean> ALLOW_HALF_CLOSURE = valueOf("ALLOW_HALF_CLOSURE");  
public static final ChannelOption<Boolean> AUTO_READ = valueOf("AUTO_READ");  
  
/**  
 * If {@code true} then the {@link Channel} is closed automatically and immediately on write failure.  
 * The default value is {@code true}.  
 */
public static final ChannelOption<Boolean> AUTO_CLOSE = valueOf("AUTO_CLOSE");  

public static final ChannelOption<Boolean> SO_BROADCAST = valueOf("SO_BROADCAST");  
public static final ChannelOption<Boolean> SO_KEEPALIVE = valueOf("SO_KEEPALIVE");  
public static final ChannelOption<Integer> SO_SNDBUF = valueOf("SO_SNDBUF");  
public static final ChannelOption<Integer> SO_RCVBUF = valueOf("SO_RCVBUF");  
public static final ChannelOption<Boolean> SO_REUSEADDR = valueOf("SO_REUSEADDR");  
public static final ChannelOption<Integer> SO_LINGER = valueOf("SO_LINGER");  
public static final ChannelOption<Integer> SO_BACKLOG = valueOf("SO_BACKLOG");  
public static final ChannelOption<Integer> SO_TIMEOUT = valueOf("SO_TIMEOUT");  
  
public static final ChannelOption<Integer> IP_TOS = valueOf("IP_TOS");  
public static final ChannelOption<InetAddress> IP_MULTICAST_ADDR = valueOf("IP_MULTICAST_ADDR");  
public static final ChannelOption<NetworkInterface> IP_MULTICAST_IF = valueOf("IP_MULTICAST_IF");  
public static final ChannelOption<Integer> IP_MULTICAST_TTL = valueOf("IP_MULTICAST_TTL");  
public static final ChannelOption<Boolean> IP_MULTICAST_LOOP_DISABLED = valueOf("IP_MULTICAST_LOOP_DISABLED");  
  
public static final ChannelOption<Boolean> TCP_NODELAY = valueOf("TCP_NODELAY");  
/**  
 * Client-side TCP FastOpen. Sending data with the initial TCP handshake. 
 */
public static final ChannelOption<Boolean> TCP_FASTOPEN_CONNECT = valueOf("TCP_FASTOPEN_CONNECT");  
/**  
 * Server-side TCP FastOpen. Configures the maximum number of outstanding (waiting to be accepted) TFO connections.   
 */
public static final ChannelOption<Integer> TCP_FASTOPEN = valueOf(ChannelOption.class, "TCP_FASTOPEN");  
  
public static final ChannelOption<Boolean> SINGLE_EVENTEXECUTOR_PER_GROUP =  
        valueOf("SINGLE_EVENTEXECUTOR_PER_GROUP");
```

可以看到当访问ChannelOption预定义的ChannelOption类型常量时，会尝试通过valueOf()来获取。由valueOf()方法知道，第一次访问ChannelOption预定义的ChannelOption类型常量时，内部维护的this.pool是没有的，this.pool会创建一个ChannelOption类型。

# 预定义常量功能

| 预定义常量   | 类型    | 功能描述                                                                                                 |
| ------------ | ------- | -------------------------------------------------------------------------------------------------------- |
| SO_RCVBUF    | Integer | TCP传输选项。设置TCP接收缓冲区的大小。                                                                   |
| SO_SNDBUF    | Integer | TCP传输选项。设置TCP发送缓冲区的大小。                                                                   |
| SO_KEEPALIVE | Boolean | TCP传输选项。是否开启TCP的心跳机制。启用时，TCP会主动探测空闲连接的有效性。默认心跳间隔7200s。默认关闭。 |
| SO_BACKLOG |         |                                                                                                          |

## SO_BACKLOG

TCP传输选项。表示服务端接收连接的队列长度，如果队列已满，客户端连接将被拒绝。

>服务端在处理客户端新连接请求时（三次握手）是顺序处理的，所以同一时间只能处理一个客户端连接，多个客户端到来的时候，**服务端将不能处理的客户端连接请求放在队列中等待处理，队列的大小通过SO_BACKLOG指定。**

1.  服务端对完成第二次握手的连接放在一个队列（暂时称A队列）
2.  如果进一步完成第三次握手，再把连接从A队列移动到新队列（暂时称B队列）
3.  接下来应用程序会通过调用accept()方法取出握手成功的连接，而系统则会将该连接从B队列移除。
4.  A和B队列的长度之和是SO_BACKLOG指定的值，当A和B队列的长度之和大于SO_BACKLOG值时，新连接将会被TCP内核拒绝。所以，如果SO_BACKLOG过小，accept速度可能会跟不上，A和B队列全满，导致新客户端无法连接。

如果连接建立频繁，服务器处理新连接较慢，那么可以适当调大这个参数。


# 方法

## valueOf()（需要补充）

返回类型：`ChannelOption<T>`
方法签名：`public static `
语法：
- `valueOf(String name)`
- `valueOf(Class<?> firstNameComponent, String secondNameComponent)`

从代码层面可以看到，ChannelOption中的valueOf()方法本质上都是调用ChannelOption中的ConstantPool的valueOf()方法，也就是尝试从ChannelOption内部维护的ConstantPool中拿到ChannelOption类型常量。至于ConstantPool的valueOf()方法，见....