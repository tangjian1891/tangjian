---
description: 基础知识
---

# node

### ArrayBuffer

ArrayBuffer 二进制数组，是对内存的连续引用，不是数组，但是可以用数组的方式操作。

1byte(1b) 就是1个字节占8位。2进制8位范围就是256

### 三个构造

1.ArrayBuffer:代表内存中有一段二进制数据。不能直接操作ArrayBuffer，要用“视图”操作

2.DataView:属于视图:可自定义格式，例如:第一个单位为Uint8，第二个单位为Uint16。（更为灵活的读写）

3.TypedArray:属于视图:一共有9中，Uint8Array,Uint16Array,Int32Array...等。（相对简单类型的读写）

#### ArrayBuffer:构造函数。参数只能是开辟的空间字节长度。

```
//参数是所需内存大小。 在内存中生成了一段32字节的内存区域，每个字节默认是0。
const buf = new ArrayBuffer(32); //内部可查看多种“视图”
```

#### DataView:视图，相当于不需要设置精准类型。动态的、。

TypedArray实际上是一组构造函数的总称,因为是固定。所以很多。 好比java的“申明”类型和javascript的“var申明”

```
const buf = new ArrayBuffer(32);//32个字节，仅仅是开辟空间而已
const dataView = new DataView(buf);//将内存放入视图中
console.log(dataView, dataView.byteLength);//object, 32
```

#### TypedArray:视图，可操作“二进制数组”。参数：接收ArrayBuffer实例作为参数||普通数组作为参数，直接分配内存底层生成ArrayBuffer实例。（强大功能）

不同的TypedArray的区别：是否有符号，还有一个就是内部的单个单位所占字节数，单个单位所在字节数

```
const buf = new ArrayBuffer(32);//32个字节，仅仅是开辟空间而已
const x1 = new Int32Array(buf);//一个单位占32位=4字节。所以有[0,0,0,0,0,0,0,0]
const x2 = new Uint8Array(buf);//一个单位占8位=1字节，所以有[0,0,0,0,0,0,0，0，...(一共32个0)]
```



#### ArrayBuffer与字符串相互转换

可使用原生方法 TextEncoder和TextDecoder做相互转化

```
//字符串转ArrayBuffer
var encoder = new TextEncoder();
var view = encoder.encode(input);
//ArrayBuffer转字符串
var decoder = new TextDecoder(outputEncoding);
decoder.decode(input);
```

### Blob

二进制大对象(binary large object),原型上有一个arrayBuffer函数，返回的对应的ArrayBuffer，可以理解为Blob基于ArrayBuffer二进制数组封装而成。而File又是继承Blob扩展而成。

```
//array是一个数组，数组中可以是ArrayBuffer，TypedArray,DataView,Blob，DOMString
//options.type 对应设置MIME
var aBlob = new Blob(array[, option]);

//实例对象上除了设置的type属性外，还有一个size属性
```



### MIME

MIME多用途互联网邮件扩展(Multipurpose Internet Mail Extensions)，被引用与多种协议中(http协议，Blob,File)[参考文章](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics\_of\_HTTP/MIME\_types)

一般都是一个主类型+一个子类型，用斜杠分割，不允许空格。比如text/plain代表纯文本.txt

| 类型          | 描述                                    | 典型示例                                              |
| ----------- | ------------------------------------- | ------------------------------------------------- |
| text        | 文件是普通文本，理论上人类可读。                      | text/plain，text/html，text/css，text/javascript     |
| image       | 图像(png,gif)                           | image/gif，image/png...                            |
| audio       | 音频                                    | audio/midi，audio/mpeg...                          |
| video       | 视频                                    | video/mp4，video/webm                              |
| application | 二进制数据。（可以理解人类直接读不明白的,像pdf文件，打开也不知道是啥） | application/pdf，application/json，application/json |

#### MIME规律:除去已定义的text,image,audio,video。剩下的不认识的都可以归到application中，因为文件打开也读不懂，被读取出来就是二进制的。

### FileReader

转化桥梁，可将File或Blob转化为Text,DataURL，以及ArrayBuffer。

```
const fr = new FileReader();
fr.onload=function(){...} //读取后的回调
fr.readAsText(Blob); //转为纯文本
fr.readAsDataURL(Blob);//DataURL,如果是二进制数据，会转为base64
```

### DataURL

前缀为data:协议的URL，允许浏览器解析其中的内容。

格式规则:       data:\[MIME]\[;base64],\<data>  或者 data:;,\<data>普通文本

| 规则         | 描述      | 是否必选。默认值                        |
| ---------- | ------- | ------------------------------- |
| data:      | 协议      | 必有                              |
| \[MIME]    | MIME的类型 | 选填。默认值:text/plain               |
| \[;base64] | 如果      | 选填。如果是非文本可用base64转换数据           |
| ,\<data>   | 数据本身    | 可能是base64,可能是urlencode的，可能是字符文本 |

注意:只有是二进制数据(例如file)才一定需要base64编码后才能在DataURL中展示。一般来说，浏览器上的数据最好urlencode一下，因为浏览器对字符有限制（像redirect回调参数这种，也最好encode一下）。

```
data:,Hello%2C%20World!
简单的 text/plain 类型数据

data:text/plain;base64,SGVsbG8sIFdvcmxkIQ%3D%3D
上一条示例的 base64 编码版本

data:text/html,%3Ch1%3EHello%2C%20World!%3C%2Fh1%3E
一个HTML文档源代码 <h1>Hello, World</h1>

data:text/html,<script>alert('hi');</script>
一个会执行 JavaScript alert 的 HTML 文档。注意 script 标签必须封闭。
```

### 表单/Aja与服务器、传参接收

#### 表单提交:

表单提交默认是get请求，没有请求body(设置enctype无效)，表单数据作为Query String Parameters参数。url上?之后的key=value，key与value都将被urlencode。

手动POST请求:会根据enctype决定使用application/x-www-form-urlencoded还是multipart/form-data。

#### ajax提交(axios)

GET请求后，自动忽略data属性中的值。如果需要Query String Parameters，则需要使用params属性，会自动转为查询参数.

POST请求,完全根据data属性的值类型动态设置，无需手动干预。&#x20;

* 如果data为字面量对象，那么"Content-type": "application/json"
* 如果data为qs.stringify库转换过的字符串,那么"Content-type"= "application/x-www-form-urlencoded"
* 如果data为FormData的实例对象，那么"Content-type"= "multipart/form-data"

#### 服务端express接收

```
req.url 是请求的路径。例如:/haha/qwer/do?address=%E5%8C%97%E4%BA%AC&age=25&file=
req.query express自动解析url上的key=value,相当于自动解析Query String Parameters

凡是请求体body中的数据，都是二进制，在不借助中间件时，需要手动接收
//   let bufArr = [];
//   await new Promise((r) => {
//     req.on("data", (e) => {
//       bufArr.push(e);
//     });
//     req.on("end", () => {
//       const buf = Buffer.concat(bufArr);
//       console.log(buf.toString()); //这里就能看到最终的数据了。 实际上和get一致，也是key=value形式,并且被urlEncode编码。
//       // 后续就需要自己放入req.body中， 一般也不会手动放入，一般也是靠中间件处理好了，咱们直接从req.body里面拿
//       r(); //结束
//     });
//   });
```

使用express-formidable中间件处理所有接收参数

```
const formidableMiddleware = require("express-formidable");
// 处理数据接收
app.use(formidableMiddleware()); //使用了中间件，就不能使用req.on('data')接收了，会被中间件提前消费掉

请求体body中的数据会被分别解析到req.fields和req.files上,query参数仍然在req.query上
```

### 前端接收文件流

```
const fm = new FormData();
// 一定要设定responseType:blob，否则浏览器当字符串解析了
axios.request({ url: "/getUpload", method: "post", responseType: "blob" }).then((res) => {
  const headers = res.headers;//文件名称在请求头中
  // 注意blob接收的是一个二进制数组，包一下
  const blob = new Blob([res.data], {
    type: "image/jpg",
  });
  const filename = decodeURI(headers["content-disposition"].split("filename=")[1])
  const href = URL.createObjectURL(blob);//转换为url
  const a = document.createElement("a");
  a.href = href;
  a.download = filename || "name.xlsx";//文件名称
  a.click();
  URL.revokeObjectURL(href);//释放内存
});
```

### Web Worker

Web Worker可以为javascript创造多线程环境。计算密集型或高延迟的任务给worker线程，主线程负责UI交互仍然流畅，不会被阻塞或拖慢 worker线程一旦创建成功，就会始终运行，不会被主线程上的活动 打断。所以创建worker线程消耗资源，在不使用的情况下，建议关闭。

1. 同源限制:worker脚本文件必须与主线程同源
2. DOM限制:worker无法读取网页DOM对象，包括document，window。 但是可以用location和navigator对象
3. 通信限制:worker线程和主线程不在同一个上下文环境，他们不能直接通信，必须通过消息完成
4. 脚本限制:不能执行alert和conform方法，但是可以发ajax请求
5. worker不能读本地文件。不能打开本机文件系统(file://),它所加载的脚本，必须来自网络

关于跨域加载

```
 //跨域加载worker.js
 var blob = new Blob(['importScripts("https://cdn.jsdelivr.net/npm/pdfjs-dist@2.0.943/build/pdf.worker.min.js")'], {
   type: "application/javascript",
 });
 var blobUrl = window.URL.createObjectURL(blob);
 let pdfjsWorker = new Worker(blobUrl);
```

使用axios：可以手动给一个this.window={}模拟，让axios挂载上
