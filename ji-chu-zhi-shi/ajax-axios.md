# ajax/axios

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

Query参数:URL中?后面得部分就是一个Query String。

Body参数:因为[URL上有长度限制](https://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers) 2000字符，敏感数据会跟随url记录到日志。Body如果是form表单，那么格式与Query其实一致

实体头部(header)中用来描述资源得MIME类型

request header 请求头中

body体不能直接接收js对象，\[object Object]，需要用URLSearchParams包裹

1. application/x-www-form-urlencoded;   “form表单”数据按key/value形式发送 ，\
   (ajax) 如果是URLSearchParams 对象,自动设置Content-type&#x20;
2. 如果是纯字符串。需要手动设置Content-type。且字符串满足key=value格式\


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



