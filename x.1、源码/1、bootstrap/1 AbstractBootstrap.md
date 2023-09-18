AbstractBootstrap是一个帮助类，提供方法来创建一个Channel并注册到一个EventLoop中。

下面是AbstractBootstrap的定义：
```java
public abstract class AbstractBootstrap<B extends AbstractBootstrap<B, C>, C extends Channel> implements Cloneable
```

# 属性

## 1）this.group（需要补充）

```java
volatile EventLoopGroup group;
```
1）group(...)
语法：`B group(EventLoopGroup group)`
补充：
- 传入的group不能为空。
- 如果group属性已经设置了，再次通过这个方法来设置group属性会抛出异常。
-- --
## this.localAddress

下面是定义：
```java
private volatile SocketAddress localAddress;
```

功能：用在bind()方法中，将Channel绑定到默认的this.localAddress。

设置this.localAddress。
- `public B localAddress(int inetPort)`
- `public B localAddress(String inetHost, int inetPort`
- `public B localAddress(InetAddress inetHost, int inetPort)`
- `public B localAddress(SocketAddress localAddress)`

## this.channelFactory

下面是定义：
```java
@SuppressWarnings("deprecation")  
private volatile ChannelFactory<? extends C> channelFactory;
```
功能：使用该ChannelFactory来实例化Channel实例。
1）channel()
语法：`public B channel(Class<? extends C> channelClass)`
描述：
设置的ChannelFactory类型是ReflectiveChannelFactory，ReflectiveChannelFactory使用反射获取传入的channelClass的<font color=44cf57>无参构造</font>，当需要创建Channel实例时，使用该无参构造来实例化Channel。因此，<font color=44cf57>这个方法适用于Channel实现类有无参构造</font>。
2）channelFactory
语法：`public B channelFactory(io.netty.channel.ChannelFactory<? extends C> channelFactory)`

## this.handler（需要补充）

下面是定义：
```java
private volatile ChannelHandler handler;
```
1）handler()
语法：**`B handler(ChannelHandler handler)`**
-- --
## this.options && this.attributes

下面是定义：
```java
private final Map<ChannelOption<?>, Object> options = new LinkedHashMap<ChannelOption<?>, Object>();
private final Map<AttributeKey<?>, Object> attrs = new ConcurrentHashMap<AttributeKey<?>, Object>();

```
1）option()
语法：**`<T> B option(ChannelOption<T> option, T value)`**
功能：
- 当value不为null时，向this.options中添加 option : value 键值对。
- 当value为null时，从this.options中移除key键。
2）attr()
语法：**`<T> B attr(AttributeKey<T> key, T value)`**
功能：
- 当value不为null时，向this.attrs中添加 key : value 键值对。
- 当value为null时，从this.attrs中移除key键。






# 方法

## register()

## bind()

功能：创建一个Channel，并将它绑定到一个SocketAddress。
返回类型：ChannelFuture
语法：
- `bind()`：创建一个Channel，并将它绑定到this.localAddress。
- `bind(int inetPort)`
- `bind(String inetHost, int inetPort)`
- `bind(InetAddress inetHost, int inetPort)`
- `bind(SocketAddress localAddress)`



## validate()


# 私有方法

## doBind()（重要理解）

bind()方法的主要逻辑主体就是doBind()。下面来看该方法：
```java
private ChannelFuture doBind(final SocketAddress localAddress) {  
    final ChannelFuture regFuture = initAndRegister();  // 1 
    final Channel channel = regFuture.channel();  
    if (regFuture.cause() != null) {  
        return regFuture;  
    }  
    if (regFuture.isDone()) {  
        // At this point we know that the registration was complete and successful.  
        ChannelPromise promise = channel.newPromise();  
        doBind0(regFuture, channel, localAddress, promise);  
        return promise;  
    } else {  
        // Registration future is almost always fulfilled already, but just in case it's not.  
        final PendingRegistrationPromise promise = new PendingRegistrationPromise(channel);  
        regFuture.addListener(new ChannelFutureListener() {  
            @Override  
            public void operationComplete(ChannelFuture future) throws Exception {  
                Throwable cause = future.cause();  
                if (cause != null) {  
                    // Registration on the EventLoop failed so fail the ChannelPromise directly to not cause an  
                    // IllegalStateException once we try to access the EventLoop of the Channel.                    
                    promise.setFailure(cause);  
                } else {  
                    // Registration was successful, so set the correct executor to use.  
                    // See https://github.com/netty/netty/issues/2586                    
                    promise.registered();  
                    doBind0(regFuture, channel, localAddress, promise);  
                }  
            }  
        });  
        return promise;  
    }  
}
```
1：initAndRegister()


initAndRegister()方法：
- 由子类重写的init(Channel)方法完成新建Channel实例的初始化工作。
- 
```java
final ChannelFuture initAndRegister() {  
    Channel channel = null;  
    try {  
        channel = channelFactory.newChannel();  
        init(channel);  // 2 该方法是一个protected方法，必须被子类重写
    } catch (Throwable t) {  
        if (channel != null) {  
            // channel can be null if newChannel crashed (eg SocketException("too many open files"))  
            channel.unsafe().closeForcibly();  
            // as the Channel is not registered yet we need to force the usage of the GlobalEventExecutor  
            return new DefaultChannelPromise(channel, GlobalEventExecutor.INSTANCE).setFailure(t);  
        }  
        // as the Channel is not registered yet we need to force the usage of the GlobalEventExecutor  
        return new DefaultChannelPromise(new FailedChannel(), GlobalEventExecutor.INSTANCE).setFailure(t);  
    }
    ChannelFuture regFuture = config().group().register(channel);  
    if (regFuture.cause() != null) {  
        if (channel.isRegistered()) {
            channel.close();
        } else {  
            channel.unsafe().closeForcibly();  
        }  
    }  
  
    // If we are here and the promise is not failed, it's one of the following cases:  
    // 1) If we attempted registration from the event loop, the registration has been completed at this point.    
    //    i.e. It's safe to attempt bind() or connect() now because the channel has been registered.    
    // 2) If we attempted registration from the other thread, the registration request has been successfully    
    //    added to the event loop's task queue for later execution.    
    //    i.e. It's safe to attempt bind() or connect() now:    
    //         because bind() or connect() will be executed *after* the scheduled registration task is executed    
    //         because register(), bind(), and connect() are all bound to the same thread.  
    return regFuture;  
}
```

