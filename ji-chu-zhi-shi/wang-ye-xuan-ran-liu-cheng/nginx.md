# nginx

windows

```
tasklist /fi  "imagename eq nginx.exe"            查看nginx进程对应pid
start nginx                                       启动nginx
nginx -s quit                                     安全退出
nginx -s stop                                     直接退出
nginx -s reload                                   重载nginx.conf配置
```
