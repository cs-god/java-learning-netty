下面是HttpResponse的定义：
```java
public interface HttpResponse extends HttpMessage {
	HttpResponseStatus status();
	HttpResponse setStatus(HttpResponseStatus status);
}
```
可以看到，HttpResponse增加了响应报文中的状态码部分。