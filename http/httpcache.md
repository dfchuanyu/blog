### 1、浏览器请求流程
* 浏览器首次请求

![Alt text](http://static.oschina.net/uploads/space/2015/0119/015343_psx2_568818.png)
* 浏览器再次请求

![Alt text](http://static.oschina.net/uploads/space/2015/0119/015353_P04w_568818.png)

### 2、关于缓存的HTTP HEADERS
#### 2.1 HEADERS
* Cache-Control	      响应
* Expires	            响应
* Last-Modified	      响应
* If-Modified-Since	  请求
* ETag	              响应
* If-None-Match       请求
#### 2.2 
> Cache-Control 的参数包括:
>>* max-age=[单位：秒 seconds] — 设置缓存最大的有效时间. 类似于 Expires, 但是这个参数定义的是时间大小（比如：60）而不是确定的时间点.单位是[秒 seconds].
>>* s-maxage=[单位：秒 seconds] — 类似于 max-age, 但是它只用于公享缓存 (e.g., proxy) .
>>* public — 响应会被缓存，并且在多用户间共享。正常情况, 如果要求 HTTP 认证,响应会自动设置为 private.
>>* private — 响应只能够作为私有的缓存(e.g., 在一个浏览器中)，不能再用户间共享。
>>* no-cache — 响应不会被缓存,而是实时向服务器端请求资源。这一点很有用，这对保证HTTP 认证能够严格地禁止缓存以保证安全性很有用（这是指页面与public结合使用的情况下）.既没有牺牲缓存的效率，又能保证安全。
>>* no-store — 在任何条件下，响应都不会被缓存，并且不会被写入到客户端的磁盘里，这也是基于安全考虑的某些敏感的响应才会使用这个。
>>* must-revalidate — 响应在特定条件下会被重用，以满足接下来的请求，但是它必须到服务器端去验证它是不是仍然是最新的。
>>* proxy-revalidate — 类似于 must-revalidate,但不适用于代理缓存.

> Expires
>>* Expires是Web服务器响应消息头字段，在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求。
>>* http1.0时期的属性，现在默认浏览器均默认使用HTTP 1.1，所以它的作用基本忽略。
>>* Expires 的一个缺点就是，返回的到期时间是服务器端的时间，这样存在一个问题，如果客户端的时间与服务器的时间相差很大（比如时钟不同步，或者跨时区），那么误差就很大
>>* 所以在HTTP 1.1版开始，使用Cache-Control: max-age=秒替代

> Last-Modified/If-Modified-Since：Last-Modified/If-Modified-Since要配合Cache-Control使用。
>>* Last-Modified：标示这个响应资源的最后修改时间。web服务器在响应请求时，告诉浏览器资源的最后修改时间。
>>* If-Modified-Since：当资源过期时（使用Cache-Control标识的max-age），发现资源具有Last-Modified声明，则再次向web服务器请求时带上头 If-Modified-Since，表示请求时间。web服务器收到请求后发现有头If-Modified-Since 则与被请求资源的最后修改时间进行比对。若最后修改时间较新，说明资源又被改动过，则响应整片资源内容（写在响应消息包体内），HTTP 200；若最后修改时间较旧，说明资源无新修改，则响应HTTP 304 (无需包体，节省浏览)，告知浏览器继续使用所保存的cache。

> Etag/If-None-Match：Etag/If-None-Match也要配合Cache-Control使用。
>>* Etag：web服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）。Apache中，ETag的值，默认是对文件的索引节（INode），大小（Size）和最后修改时间（MTime）进行Hash后得到的。
>>* If-None-Match：当资源过期时（使用Cache-Control标识的max-age），发现资源具有Etage声明，则再次向web服务器请求时带上头If-None-Match （Etag的值）。web服务器收到请求后发现有头If-None-Match 则与被请求资源的相应校验串进行比对，决定返回200或304。

> 既生Last-Modified何生Etag？
>>* Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间
>>* 如果某些文件会被定期生成，当有时内容并没有任何变化，但Last-Modified却改变了，导致文件没法使用缓存
>>* 有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形
>>* 服务器会优先验证if-modified-since请求头,再验证if-none-match,但是必须要两者头通过验证的时候才返回304,其中一个验证失败,都将返回新资源和200状态;

> 参考文档
>>* [浏览器 HTTP 协议缓存机制详解](https://www.cnblogs.com/520yang/articles/4807408.html)
>>* [缓存习题](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=2653578381&idx=1&sn=3f676e2b2e08bcff831c69d31cf51c51&chksm=84b3b68ab3c43f9c9f5fc826f462494dc8457d8994c3789007b7182e3d30e86876688ea1bc8f&mpshare=1&scene=1&srcid=1215muPmHY8jyQT277grjyvE#rd)
