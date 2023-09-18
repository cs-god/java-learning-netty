HttpRequest的定义如下：
```java
public interface HttpRequest extends HttpMessage {
	HttpMethod method();
	HttpRequest setMethod(HttpMethod method);

	String uri();
	HttpRequest setUri(String uri);
}
```
可以看到，HttpRequest是对请求报文部分的请求方法和URI做了补充。