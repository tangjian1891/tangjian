# ajax/axios

### HTTP

#### cors

FAQ

检查地址是否拥有完整得**协议**，**端口号。**

观察请求中得origin是否含有。

观察是否含有preflight请求。

**Provisional headers are show 临时请求头展示-一般是origin也就是目标地址没写全**

如何开启CORS?**Acess-Control-Allow-xxx：\*   || string【】**

* Access-Control-Allow-Origin:\*或指定ip。这是最基础必备
* Access-Control-Allow-Methods:\* | string\[] &#x20;
  * 只有GET、POST、HEAD请求不需要加 （简单请求）
* Access-Control-Allow-Headers:\* | string\[] &#x20;
  * 只要有自定得headers，就需要加
  * 对于Content-Type这个header,在application/json时需要加，两个表单都不需要(简单请求)

**简单请求是没有preflight预检请求得。也就是说，CORS中Origin是必备。**

**另外还有两个是可选得，比如 预检请求得缓存时间，是否可以携带cookie**



### XMLHttpRequest

#### 1.readyState （请求状态）

默认0。  【0，1，2，3，4】

4代表请求状态得完成  与之对应得就是 onreadystatechange 事件得触发。不使用onload是因为所有浏览器都支持 onreadystatechange&#x20;

#### 2.timeout（毫秒级）

当属性设置非0后，超时自动取消。   触发对应得ontimeout事件。

#### 3.abort

如果这个请求已被发出，则立刻中止请求。    会触发对应得onabort事件

#### 4.onload

如果一个请求顺利完成，可以触发onload。 （这里肯定排除timeout于abort了)。可能会有兼容性

#### 5.setRequestHeader

设置一个请求头。（记得正open()之后，send()之前设置）

#### 6.status （响应状态）

默认0。如果出错，那么也就是0。  一般会和后端约定200（正常200），与readystate=4配合使用

#### 7.send(body?)

发送请求 send(body?: Document | XMLHttpRequestBodyInit | null): void;&#x20;

**如果请求方法是 GET 或 HEAD，则忽略该参数(没有请求体)**

可以是一个Document,发送前会被序列化

type XMLHttpRequestBodyInit = Blob | BufferSource | FormData | URLSearchParams | string;



### Content-Type

理清一下参数携带相关得

#### Query参数

* URL中?后面得部分就是一个Query String。  （所有请求都可以有）形式key=value，以&连接，相同key会放到数组中。 可以使用URLSearchParams实例得toString() 获取

#### Body参数

1. 扩展URL携带参数长度限制。
2. url可能记录到日志，敏感数据。
3. GET,HEAD请求自动忽略body。
4. 在不手动使用Content-Type得情况下。
   1. 字符串-->"字符串"
   2. js对象/数组-->\[object Object]
   3. URLSearchParams实例对象-->自动转为form表单，key=value形式发送，且将Content-Type自动设置为application/x-www-form-urlencoded
   4. FormData实例对象-->自动转为multipart/form-data,可以用来传递媒体文件。
5. 手动给Content-Type情况下，需要正确得body体字符格式才能解析。
   1. Content-Type:application/x-www-form-urlencoded  需要形式为key=value,以&连接得字符格式，form表单形式。可以使用URLSearchParams.prototype.toString()获取，旧的工程项目可以查看是否有qs包,qs.stringify()也行。
   2. Content-Type:application/json  需要JSON格式字符串，可以使用JSON.stringify()序列化
   3. multipart/form-data只能是FormData实例
6. form表单默认是application/x-www-form-urlencoded，可以修改enctype属性为form-data







\


#### application/json;&#x20;

JSON.stringify()数据，保证字符串格式为json串

如果body体是URLSearchParams 对象，则会自动设置Content-type

multipart/form-data;      媒体格式得表单。也叫“form-data”。可以在表单得enctype设置

application/json;      json数据格式

text/plain;       叫Request Payload，纯字符串





### Request Method

GET HEAD&#x20;

### 扩展:http respones status codes

100 信息响应 【100，101，102，103】

200 成功响应

* 200 OK 请求成功
* 201 Created 创建成功，算请求成功，语义化，通常是POST请求，或者PUT请求
* 204 No Content 没内容，标头有用，例如:option预请求

300 重定相关 [https://cloud.tencent.com/developer/article/1762070](https://cloud.tencent.com/developer/article/1762070)

* 301 Moved Permanetly 重定向
* 302 Found&#x20;
* 304 Not Modified 资源未修改，本地缓存仍然可用

400 客户端错误

* 400 Bad Request 请求参数错误
* 401 Unauthorized 未授权  未知身份，服务器拒绝
* 403 Forbidden  服务器已知身份，权限不够禁止访问
* 404 Not Found 接口地址未找到
* 405 Method Not Allowed 请求方法不被允许
* 408 Request Timeout

500 服务端错误

* 500 Internal Server Error 服务报错，没处理，抛出来了
* 503  Service Unavailable 服务不可用，系统重启得时候



