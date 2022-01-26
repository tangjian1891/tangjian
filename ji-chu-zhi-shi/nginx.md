# nginx

windows

```
tasklist /fi  "imagename eq nginx.exe"            查看nginx进程对应pid
start nginx                                       启动nginx
nginx -s quit                                     等待工作进程完成后，安全退出
nginx -s stop                                     直接退出
nginx -s reload                                   重载nginx.conf配置
```

可以start多次，进程会额外开启，但是关掉后，只能关掉最后一个批次的。其余的需要手动kill pid

```
location [ = | ~ | ~* | ^~] uri {
	...
}
```

1. `=` 精确匹配路径，用于不含正则表达式的 uri 前，如果匹配成功，不再进行后续的查找；全匹配与5匹配开头是有差异得
2. `^~` 用于不含正则表达式的 uri； 表示如果该符号后面的字符是最佳匹配，采用该规则，不再进行后续的查找；
3. `~` 表示用该符号后面的正则去匹配路径，区分大小写；
4. `~*` 表示用该符号后面的正则去匹配路径，不区分大小写。跟 `~` 优先级都比较低，如有多个location的正则能匹配的话，则使用正则表达式最长的那个；
5. 没有前缀得话，则匹配/开头符合，则命中整体url。

如果 uri 包含正则表达式，则必须要有 `~` 或 `~*` 标志。

例如:

关于nginx的http.server.location配置，凡是以/结尾的，都是访问的文件夹，默认查询对应root下的index开头的文件，index.html index.htm index.php

路径寻找拼接规则: root+location



### 路径后面自动/说明

关于URL尾部/说明。仅针对有pathname得路径，至少是www.baidu.com/tieba。 www.baidu.com这种nginx访问/得不采用以下规则。&#x20;

1. 首先根据约定URL尾部有/代表目录。会自动查找目录下得index.html等文件。
2. 如果没有/代表用户明确想要得是文件。但是可能没有文件，此时会做目录检测，如果有目录且有index.html，那么就会返回，并且将浏览器上URL加上/



