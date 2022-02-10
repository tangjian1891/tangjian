# nginx/pm2

## windows/nginx

```
tasklist /fi  "imagename eq nginx.exe"            查看nginx进程对应pid
start nginx                                       启动nginx
nginx -s quit                                     等待工作进程完成后，安全退出
nginx -s stop                                     直接退出
nginx -s reload                                   重载nginx.conf配置
```

可以start多次，进程会额外开启，但是关掉后，只能关掉最后一个批次的。其余的需要手动kill pid

### 静态资源部署

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

{% hint style="info" %}
路径寻找拼接规则: root+location
{% endhint %}

### 路径后面自动/说明

关于URL尾部/说明。仅针对有pathname得路径，至少是www.baidu.com/tieba。 www.baidu.com这种nginx访问/得不采用以下规则。&#x20;

1. 首先根据约定URL尾部有/代表目录。会自动查找目录下得index.html等文件。
2. 如果没有/代表用户明确想要得是文件。但是可能没有文件，此时会做目录检测，如果有目录且有index.html，那么就会返回，并且将浏览器上URL加上/

### 单页面history模式

为什么刷新一次就会404呢?

因为按照url来说，确实无法匹配对应的路径文件，所以必须让nginx重新寻找我们的单页面index.html，匹配后vue-router会自动解析history。[配置文档](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90)

```
location    / {
  root html/www/dist;
  try_files $uri $uri/ /index.html;
}
//一般项目都是部署在非根路径下
location    /h5 {
  root html/www/dist;
  try_files $uri $uri/ /index.html;
}
```

## linux/nginx

```
yum install -y zlib-devel openssl-devel pcre gcc            必要环境
mkdir -p /home/soft/nginx                                创建目录
cd /home/soft/nginx                                进入目录
wget http://nginx.org/download/nginx-1.20.2.tar.gz
tar -zxvf nginx-1.20.2.tar.gz
#此时你会发现没有sbin目录，所以无法启动,需要设置prefix路径，安装
cd /home/soft/nginx/nginx-1.20.2                进入目录
./configure --prefix=/home/soft/nginx/nginx-1.20.2        设置前缀
make && make install
cd /home/soft/nginx/nginx-1.20.2/sbin        
./nginx -v                    即可查看当前nginx的版本(此时还要使用相对路径)

#将nginx添加到全局变量中，随时使用nginx指令
ln -s /home/soft/nginx/nginx-1.20.2/sbin/nginx    /usr/local/bin     随时使用nginx

可能会需要防火墙      centOS7关闭防火墙命令： systemctl stop firewalld.service
```

查看是否启动成功

ps -ef | grep nginx

![](<../.gitbook/assets/image (2).png>)

```
ps -ef | grep nginx            查看nginx是否有启动
nginx                            启动nginx
nginx -s quit                                     等待工作进程完成后，安全退出
nginx -s stop                                     直接退出
nginx -s reload                                   重载nginx.conf配置

kill -9 18854            杀掉进程端口号，可能要杀掉不止一个master worker等等
```

## PM2

pm2是一个全局服务，可以帮助启动node应用程序。功能强大，统一管理。[速查文档](https://pm2.keymetrics.io/docs/usage/quick-start/)

所有启动得应用程序，会有历史记录存在，可以方便下次直接启动。可以为同一个应用程序创建多个name启动记录，但是实际只能启动一个。

```
pm2 ls                显示所有进程状态，所有得历史记录
pm2 start app.js       通过指定得路径， 启动/守护/监视 应用程序
pm2 start 0            通过历史记录列表中得id  启动/守护/监视 应用程序

pm2 stop 0     通过id停止指定得proccess
pm2 stop all

pm2 delete 0    通过id删除指定得proccess记录
pm2 delete all

pm2 reload all    realod一下所有
pm2 restart 0     通过id重启指定得process

pm2 monit        开启监听面板，可以查看服务得内存,cpu等情况

//启动cli也可以接受一些选项参数
--name <app_name>    启动服务所对应得name，帮助区分不同得项目

```

