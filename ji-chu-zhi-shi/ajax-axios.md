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

扩展:http respones status codes

100 信息响应 【100，101，102，103】

200 成功响应

* 200 OK 请求成功
* 201 Created 创建成功，算请求成功，语义化，通常是POST请求，或者PUT请求

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



