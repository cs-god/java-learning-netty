
# 定义

下面是ChannelHandler的定义：
```java
/**  
 * Gets called after the {@link ChannelHandler} was added to the actual context and it's ready to handle events.  
 */
 void handlerAdded(ChannelHandlerContext ctx) throws Exception;  
  
/**  
 * Gets called after the {@link ChannelHandler} was removed from the actual context and it doesn't handle events  
 * anymore.
 */
 void handlerRemoved(ChannelHandlerContext ctx) throws Exception;
```


# ChannelHandlerAdapter



