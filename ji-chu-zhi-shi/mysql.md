# mysql

## centos/linux

去官网下载社区下载对应版本,mysql->DOWNLOADS->MYSQL Community(GPL) Downloads->MySQL Community Server->选择**Red Hat Enterprise Linuxt/Oracle Linux**

下载[mysql-8.0.28-1.el7.x86\_64.rpm-bundle.tar](https://dev.mysql.com/downloads/mysql/)

```
#检验mariadb包，卸载mariadb
rpm -qa | grep mariadb            #查询包名
rpm -e mariadb-libs-5.5.65-1.el7.x86_64 --nodeps            #根据包名删除
cd /usr/local/ && mkdir mysql && cd mysql                #创建mysql文件目录并进入

#将下载得安装包传递到此目录下
tar -xvf mysql-8.0.28-1.el7.x86_64.rpm-bundle.tar            #会有多个rpm包
#解压后会有很多rpm包，我们要依次安装，提前替换好文件名称,基本上就是版本号得替换
rpm -ivh mysql-community-common-8.0.28-1.el7.x86_64.rpm --nodeps --force 	 安装common
rpm -ivh mysql-community-libs-8.0.28-1.el7.x86_64.rpm --nodeps --force		安装libs
rpm -ivh mysql-community-client-8.0.28-1.el7.x86_64.rpm --nodeps --force 	安装client
rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm --nodeps --force  	安装server
#查看mysql包
rpm -qa | grep mysql            #可以看到对应得4个

#开始初始化mysql
yum -y install numactl
mysqld --initialize;
chown mysql:mysql /var/lib/mysql -R;
systemctl start mysqld.service;
systemctl enable mysqld;

#查看密码，通过初始化密码登录修改后，才能被远程连接
cat /var/log/mysqld.log | grep password		#A temporary password is generated for root@localhost: QD?2u!s:vdMd
mysql -uroot -p
#先reset一下密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root123';    #修改密码
#可能不需要 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pw123';	#密码就是pw123

#还需要允许除localhost之外得ip地址连接
mysql -u root -p
use mysql;
update user set host = '%' where user = 'root';            #更新字段
select host, user from user;                    #查询一下
flush privileges;                            #刷新一下即可

```

**注意:推荐使用Navicat16+以上得版本连接，否则还会出现加密方式不一致无法连接问题**

[linux/mysql密码忘记](https://www.cnblogs.com/black-fact/p/11613361.html)

## Navicat16免费过期刷新
