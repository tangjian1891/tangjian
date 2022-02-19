# 微信开发

### 快速索引

| 分类       | 链接                                                                                                                       | 描述                             |
| -------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------ |
| 微信公众平台   | [https://mp.weixin.qq.com/](https://mp.weixin.qq.com)                                                                    | 公众号分为4种->订阅号、服务号、小程序、企业微信（企业号） |
| 微信开放平台   | [https://open.weixin.qq.com/](https://open.weixin.qq.com)                                                                | 开发人员。移动端，网站等                   |
| 微信支付     | [https://pay.weixin.qq.com/](https://pay.weixin.qq.com)                                                                  | 还没用过                           |
| 开发文档（总览） | [https://developers.weixin.qq.com/doc/](https://developers.weixin.qq.com/doc/)                                           |                                |
| 开发交流专区   | [https://developers.weixin.qq.com/community/develop/mixflow](https://developers.weixin.qq.com/community/develop/mixflow) |                                |
| 公众号FAQ   | [https://kf.qq.com/product/weixinmp.html](https://kf.qq.com/product/weixinmp.html)                                       |                                |

### 认证判断

&#x20;账号主体中会有认证V，点击去会有审查时间。认证后，可以申请额外得接口功能。除了特别主体，认证一年/300

[https://kf.qq.com/faq/1612197nEJNR161219BB7Jne.html](https://kf.qq.com/faq/1612197nEJNR161219BB7Jne.html)

### 订阅号 /服务号

1.订阅号可以是“个人”,"企业"，但是大部分都是个人。服务号只能是"企业"

2.如果想要使用高级接口功能。例如:"支付"，需要选择服务号

3.区分方式。1.设置中查看是否能“置顶”，“置顶”得就是公众号，否则为“星标”得就是订阅号。2.“订阅号”在订阅号消息列表中，“服务号”直接在聊天列表中。

{% embed url="https://kf.qq.com/faq/170815aUZjeQ170815mU7bI7.html" %}

### 登录“邮箱已被占中”

公众开放平台、公众平台（下属细化4种）、 个人微信。 一个邮箱只能被其中一个绑定。[邮箱绑定个数](https://kf.qq.com/faq/120911VrYVrA140428naUJVv.html)&#x20;



## 开始接入公众号后端接口

1.在“设置与开发”-> “基本配置”->(勾选成为开发者)->配置URL+Token(开发者定义加密串)

点击保存时，会像URL发一条根路径/的get请求，附带对应参数，需要将参数中的echostr返回。接口响应后保存成功。

```
# 消息加解密:明文
{
  signature: '3a57e547a923be185b0f97b24c92f46dddd103d9',
  echostr: '6783157876945828351',
  timestamp: '1645250738',
  nonce: '2124551008'
}
```

### Access token

access是公众号唯一接口调用凭据，调用各接口都需要access\_token，至少512字符空间。有效期2小时。

使用appid与appsecret调用接口来获取access\_token。注意:1.开启接口白名单 2."设置与开发"->"基本配置"中获取。
