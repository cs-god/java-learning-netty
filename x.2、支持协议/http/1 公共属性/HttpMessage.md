HttpMessage是一个接口，包含请求和响应报文的公共属性：版本号和报文头。

下面是HttpMessage接口的定义：
```java
public interface HttpMessage extends HttpObject {
	HttpVersion protocolVersion();
	HttpMessage setProtocolVersion(HttpVersion version);
	HttpHeaders headers();
}
```
**`protocolVersion()`**：获取版本号。
**`setProtocolVersion()`**：设置版本号。
**`headers()`**：获取请求头。

