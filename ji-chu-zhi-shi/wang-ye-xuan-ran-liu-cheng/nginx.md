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



关于nginx的http.server.location配置，凡是以/结尾的，都是访问的文件夹，默认查询对应root下的index开头的文件，index.html index.htm index.php

路径寻找拼接规则: root+location
