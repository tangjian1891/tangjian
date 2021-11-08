# web安全/攻击

### xss

跨站脚本攻击cross-site-scripting。因为缩写与css样式层叠表重复，所以只能叫xss了。

主要手段:通过漏洞在用户的浏览器内运行非法的本站HTML标签或script脚本

#### 1.反射型

也叫非持久化，一次性，不会入库。主要是利用攻击链接，攻击者对链接的查询参数进行修改。?searchKey=\<scrip...。

新版本的chrome如果发现dom.innerHTML含有脚本，那么将不执行。

```

dom.innerHTML = `<img src="/404" onerror="alert('图片XSS')">`; //可以执行
```

#### 2.存储型

也叫持久化，数据会入库。常见为表单控件的数据录入，主要由于后端未对数据做escap转义，会导致所有的用户页面造成脚本触发。

```
  function escape(str) {
    str = str.replace(/&/g, "&amp;");
    str = str.replace(/</g, "&lt;");
    str = str.replace(/>/g, "&gt;");
    str = str.replace(/"/g, "&quto;");
    str = str.replace(/'/g, "&#39;");
    str = str.replace(/`/g, "&#96;");
    str = str.replace(/\//g, "&#x2F;");
    return str;
  }
```
