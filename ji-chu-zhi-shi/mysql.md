# mysql

## centos/linux

```
#检验mariadb包，卸载mariadb
rpm -qa | grep mariadb            #查询包名
rpm -e mariadb-libs-5.5.65-1.el7.x86_64 --nodeps            #根据包名删除
cd /usr/local/ && mkdir mysql && cd mysql                #创建mysql文件目录并进入

将下载得安装包传递到此目录下
tar -xvf mysql-8.0.28-1.el7.x86_64.rpm-bundle.tar	     



```
