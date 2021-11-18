# 浏览器相关

> (抛开加载完成的先后顺序)无论是css还是js，请求的发出都是并发的，但是资源解析执行时，一定是根据顺序来的(而且阻塞)，后面的资源即使先到也要等前面的资源完全拿到并且解析完成。

### css加载

css脚本是并发请求(页面上有多少就并发多少)

(加载时)不会阻塞DOM解析生成DOM树。（但是由于不能生成rendertree，所以页面还是白屏）

(加载时)会阻塞后续js执行(即使js资源已经加载完成)

#### rel(relationship)

所有没有rel属性的link标签不会发出请求

preload与prefetch

preload:预加载，但不会阻塞渲染引擎。(但是从另外一个角度看，会占用请求数与网速资源)

prefetch：预请求，在空闲时才会发出请求加载资源，所以不会抢占必要资源的请求(符合实际业务情况,且能加载的资源包括style,script,video等)

rel设置为preload与prefetch后，还需要设置as属性确定资源类型.

### js加载

普通的script脚本加载时会阻塞渲染引擎,执行时也会阻塞渲染引擎。

script脚本是并发请求(页面上有多个就并发多少个)，但是执行顺序会根据发出请求顺序执行。

#### async和defer(加载时都不会阻塞渲染引擎(与普通script差异特点))

async异步执行:哪个js脚本加载到就会立刻执行。

defer延迟执行：会在页面dom与css加载解析，且所有css资源加载后，按照顺序执行js。

总结:所有script顺序放在\<body>结束标签之上与所有script加上defer表现一致。90%的业务情况都用不到async，因为加载到后阻塞渲染引擎，导致页面白屏。



### encodeURL与encodeURIComponent

总结:

1.访问的地址不要用encodeURIComponent加密，会导致地址中的:/被转义，浏览器无法访问(https://www.baidu.com/)

2.后缀query参数上如果有特殊符号，需要使用encodeURIComponent转义，比如回调参数地址,不转义则会被浏览器解析。（key=你好\&returnURL=https://www.baidu.com/）

```
// encodeURL不会转义;,/?:@&=+$ ,这些存在于链接之上.所以对整个链接进行 encodeURIComponent 会导致无法访问,需要手动解码才行
// 对于链接地址中的字符，我们希望被浏览器解析。https://www.baidu.com/
// 对于query参数，我们希望不被解析。
let baidu = "https://www.baidu.com/";
console.log(encodeURI(baidu)); //https://www.baidu.com/
console.log(encodeURIComponent(baidu)); //https%3A%2F%2Fwww.baidu.com%2F  把: / 转义了，导致浏览器无法识别
console.log("-----------------------------");
let baiduQuery = "https://www.baidu.com?key=你好&v=https://www.baidu.com/";
console.log(encodeURI(baiduQuery)); //这种会导致 query参数丢失
console.log(encodeURIComponent(baiduQuery)); //这种会导致整体url无法解析

// 最佳做法:仅对?后面query参数做统一encodeURIComponent，获取到后，统一decodeURIComponent即可。如果愿意，你可以对query参数的单个value使用decodeURIComponent
let extraQuery = "key=你好&v=https://www.baidu.com/";
let encodeQuery = encodeURIComponent(extraQuery);
console.log(`${baidu}?${encodeQuery}`); //最佳做法，拼接而成
```
