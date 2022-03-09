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

默认0





