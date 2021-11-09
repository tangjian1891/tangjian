# web安全/防御/攻击

### xss

跨站脚本攻击cross-site-scripting。因为缩写与css样式层叠表重复，所以只能叫xss了。主要手段:通过漏洞在用户的浏览器内运行非法的本站HTML标签或script脚本

[vue安全介绍](https://cn.vuejs.org/v2/guide/security.html)xss/csrf重点在后端，前端防不住。xss的innerHTML重点在服务端渲染出现。 包括但不限于与script，a,img标签，以及包含的未知a链接，链接的防御可以通过[sanitize-url](https://github.com/braintree/sanitize-url#readme),文本的输入可以通过[js-xss](https://github.com/leizongmin/js-xss),或者使用下面的escape转义字符串。

#### 1.反射型

也叫非持久化，一次性，不会入库。主要是利用攻击链接，诱导用户点击，攻击者对链接的查询参数进行修改。?searchKey=\<scrip...。

特别是服务端渲染的项目，更容易受到侵害。高版本的innerHTML会自动过滤script标签，所以客户端渲染来说，会有简单防御。但是像a标签,img标签，innerHTML则无能为力.

```
<a onmouseover=alert(document.cookie)>click me!</a>
`<img src="/404" onerror="alert('图片XSS')">`; //可以执行
```

#### 2.存储型

也叫持久化，数据会入库。常见为表单控件的数据录入，主要由于后端未对数据做escap转义，会导致所有的用户页面造成脚本触发。

### xss解决办法:

黑名单:符合的全部转义,适用于客户端用户的输入,转义url参数

```
function escape(str) {
  // https://www.w3school.com.cn/html/html_entities.asp  HTML实体 推荐使用实体编号
  str = str.replace(/&/g, "&#38;");
  str = str.replace(/</g, "&lt;");
  str = str.replace(/>/g, "&gt;");
  str = str.replace(/"/g, "&#34;");
  str = str.replace(/'/g, "&#39;");
  str = str.replace(/`/g, "&#96;");
  str = str.replace(/\//g, "&#x2F;");
  return str;
}
```

白名单：自定义规则保留一部分属性，例如处理富文本，保留标签样式.推荐使用js-xss

### xss注意点:

1. 服务端渲染注意清洗渲染的html字符串。&#x20;
2. 客户端由于innerHTML的帮助，不太担心script标签，但是需要注意使用img,a标签中的onerro,onload,href等js执行事件。
3. 启用CSP内容安全策略(Content Security Policy)。同源加载资源，只允许HTTPS等
4. Cookie 设置 HttpOnly ，防御登录态被js盗取

> 用户输入的文本需要进行过滤再入库。除非可信，不然不可贸然渲染到页面中。

### CSRF

跨站请求伪造(Cross Site Request Forgery)利用用户已登录的身份，在用户不知情的情况下，利用用户登录状态完成非法操作

特点:
