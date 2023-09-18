netty通过提供http协议解码和编码器以及诸多其他类来支持http协议。

io.netty.handler.codec.http包有下面接口：


HttpObject


LastHttpContent


FullHttpMessage


FullHttpRequest


FullHttpResponse

CompressionEncoderFactory

io.netty.handler.codec.http包有下面类：

----Headers----
CombinedHttpHeaders
DefaultHttpHeaders
EmptyHttpHeaders
ReadOnlyHttpHeaders

HttpHeadersEncoder

HttpHeaderValidationUtil

----Message----
DefaultHttpMessage

HttpMessageDecoderResult

HttpMessageUtil

----Content----
CombinedLastHttpContents
DefaultHttpContent
DefaultLastHttpContent

HttpContentCompressor
HttpContentDecompressor
HttpContentDecoder
HttpContentEncoder

----HttpMethod----
HttpMethod

----HttpObject----
DefaultHttpObject

HttpObjectAggregator

HttpObjectDecoder
HttpObjectEncoder

----Request----
DefaultFullHttpRequest
DefaultHttpRequest

HttpRequestEncoder
HttpRequestDecoder

----Response----
DefaultFullHttpResponse
DefaultHttpResponse

HttpResponseDecoder
HttpResponseEncoder

HttpResponseStatus

HttpStatusClass

HttpChunkedInput

HttpConstants
HttpScheme
HttpVersion

HttpExpectationFailedEvent

HttpClientCodec
HttpClientUpgradeHandler

HttpServerCodec
HttpServerExpectContinueHandler
HttpServerKeepAliveHandler
HttpServerUpgradeHandler

QueryStringDecoder
QueryStringEncoder

HttpUtil

TooLongHttpContentException
TooLongHttpHeaderException
TooLongHttpLineException


# 请求报文对应关系

