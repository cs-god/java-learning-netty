netty的Future接口是对jdk中Future接口的补充。下面是netty中Future接口的定义：
```java
public interface Future<V> extends java.util.concurrent.Future<V> {
	boolean isSuccess();
	Throwable cause();

	
}
```

netty中Future接口针对原生jdk中Future补充的方法主要是下面几类：
- isCancelable()：任务能否被取消。
- 添加和移除监听器。
- 状态方法。
- 同步获取结果的方法。

# 状态方法

因为jdk提供的确定导致任务完成的原因不够全面，见 # 

结合jdk中Future提供的方法，可以完全确定一个任务的状态及导致状态的原因：
| 状态                    | 描述                                    | 确定方法                  |
| ----------------------- | --------------------------------------- | ------------------------- |
| uncompleted             | 一个任务的初始状态，表示任务没有完成。  | isDone()返回false。       |
| completed & succeed     | 一个任务因正常完成而转入completed状态。 | isDone() && isSucess()    |
| completed & isCancelled | 一个任务因被取消而转入completed状态。   | isDone() && isCancelled() |
| completed & exception   | 一个任务因发生异常而转入completed状态。 | isDone() && cause()       |

# 添加和移除监听器

向Future添加监听器：当Future完成，即isDone()时，监听器被通知，如果任务已经完成，那么添加的监听器会被立刻通知。

- `Future<V> addListener(GenericFutureListener<? extends Future<? super V>> listener)`：添加一个监听器。
- `Future<V> addListeners(GenericFutureListener<? extends Future<? super V>>... listeners)`：添加若干监听器。

从Future中移除监听器：当Future完成，即isDone()时，被移除的监听器不再被通知，如果打算移除的监听器本身就没有和这个Future关联，那么移除方法什么都不做。

- `Future<V> removeListener(GenericFutureListener<? extends Future<? super V>> listener)`：移除一个监听器。
- `Future<V> removeListeners(GenericFutureListener<? extends Future<? super V>>... listeners)`：移除若干监听器。

# 同步获取结果

