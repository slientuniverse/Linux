@[toc]
> 上一篇写了mysql快速离线安装，我在电脑上的另一台虚拟机上安装了mysql，两台mysql可以配置成mysql主从复制了
>
> 我是一边写文档，一边配置， 这边文档写完了，我就以为虚拟机那边执行过了，搞的总是出错，心态爆炸了，爆炸了！在自己的云服务器上安装的痛快多了，一遍成，反而在自己家的虚拟机弄了半天。。。
>
> 那些写各种教程大神们真是太伟大了，我这才刚开始写，就感觉写文档真的好费劲，一分心就出问题



# Mysql主从复制（使用binlog）
```
这种方式的原理就是主服务器上的二进制日志在从服务器上再执行一遍
```



## 1.准备两台安装了mysql的虚拟机

准备两台可以连接的虚拟机或者云服务器

> *Linux：CentOS 7.7*
> *Mysql：5.7.28*
> *Master IP：192.168.91.3*
> *Slave IP：192.168.91.4*



## 2.修改数据库配置

①修改master数据库的my.conf文件

```shell
vim /etc/my.cnf
```

添加以下配置

```shell
log-bin=mysql-bin ##开启mysql二进制日志binlog
server-id=3 ##配置唯一的server-id 
```

![](E:\Project\md\image\master配置.jpg)

②修改slave数据库的my.conf文件

```shell
vim /etc/my.cnf
```

添加以下配置

```shell
relay-log=mysql-relay ##开启中继日志
server-id=4 ##配置唯一的server-id
```

![](E:\Project\md\image\slave配置.jpg)

③重启master和slave的mysql服务

```shell
service mysql restart
```



## 3.创建一个拥有复制master数据库权限的账号

①进入mysql

```shell
mysql -u root -p
```

创建账号

```mysql
GRANT REPLICATION SLAVE ON *.* TO 'master'@'192.168.91.4' IDENTIFIED BY 'master';
```

②刷新

```mysql
flush privileges;
```

③查询master的状态

```mysql
show master status\G
```

记下红框中File和Position的值，接下来slave数据库会用到，记录完就不要再操作master，以防值发生变化

![](E:\Project\md\image\master状态.jpg)



## 4.slave数据库连接master数据库

①进入mysql

```
mysql -u root -p
```

②输入mysql命令

> master_host填maser数据库的ip
>
> master_user填创建的账号
>
> master_password填创建账号的密码
>
> master_log_file填master状态里File的值
>
> master_log_pos填master状态里Position的值

```mysql
CHANGE MASTER TO master_host = '192.168.91.3',
 master_user = 'master',
 master_password = 'master',
 master_log_file = 'mysql-bin.000001',
 master_log_pos = 602;
```

![](E:\Project\md\image\salve连接master.jpg)

③开启复制

```mysql
start slave;
```

![](E:\Project\md\image\开启复制.jpg)

④查看复制装填

```mysql
SHOW SLAVE STATUS\G
```

![](E:\Project\md\image\slave状态1.jpg)

![](E:\Project\md\image\slave状态2.jpg)



## 5.测试主从复制知否成功

①查看master数据库

```mysql
show databases;
```

![](E:\Project\md\image\master数据库.jpg)

②查看slave数据库

```mysql
show databases;
```

![](E:\Project\md\image\salve数据库.jpg)

③在master上创建数据

```mysql
create database master;
use master;
create table test(id int, number int);
insert into test values (1, 2);
```

![](E:\Project\md\image\master插入数据.jpg)

④在slave数据库上验证是否复制成功

![](E:\Project\md\image\从数据库验证.jpg)



## 6.主从复制成功！

这样一个简单的mysql主从复制功能就实现了

接下来会使用mysql cluster的方式