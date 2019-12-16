[TOC]

> 家里电脑上的虚拟机都重新安装了，开始重新安装mysql

[本文csdn连接](https://blog.csdn.net/sinat_32602455/article/details/103554241)



# Linux安装mysql

## 1.下载mysql

[mysql下载链接](https://dev.mysql.com/downloads/mysql/)

> 我这里下载的是5.7.28版本的mysql

![image-20191215203515343](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215203515343.png)

![image-20191215203542977](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215203542977.png)

或者使用wget命令下载

```shell
wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz
```



## 2.安装mysql

①解压mysql安装包

```shell
cd /usr/local/
tar -zxvf mysql-5.7.28-linux-glibc2.12-x86_64.tar.gz
```

②解压完成后修改mysql目录名

```shell
mv mysql-5.7.28-linux-glibc2.12-x86_64 mysql
```

③进入mysql安装目录，创建数据文件夹

```shel
cd mysql
mkdir data
```

④创建mysql用户和用户组，修改mysql目录拥有者

```shell
groupadd mysql
useradd -r -g mysql mysql
chown -R mysql:mysql /usr/local/mysql/
chmod 755 /usr/local/mysql/
```

⑤初始化mysql

进入bin目录

```shell
./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
```

安装可能会出现以下错误，说明libaio

```shell
./mysqld: error while loading shared libraries: libnuma.so.1: cannot open shared object file: No such file or directory 
```

```shell
yum -y install libaio-devel.x86_64
yum -y install numactl
```

再次执行初始化命令成功后，输出日志，保存日志中生成的临时密码！！！



![image-20191215210143734](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215210143734.png)

⑥修改mysql配置文件，使用vim编辑mysql配置文件

```
vim /etc/my.cnf
```

下面是mysql的简易配置

![image-20191215213004684](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215213004684.png)



⑦启动mysql

回到mysql目录，启动mysql.serverr，返回SUCCESS代表成功

```shell
./support-files/mysql.server start
```

![image-20191215213213236](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215213213236.png)

⑧添加mysql服务

添加软连接

```
ln -s /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql
```

使用mysql服务重启mysql

```shell
service mysql restart
```

![image-20191215213701376](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215213701376.png)

⑨进入mysql

```shell
mysql -u root -p
```

密码为生成的临时密码，然后修改密码

```mysql
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');
```

![image-20191215214326108](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215214326108.png)



退出mysql，使用新密码登陆

```mysql
exit;
mysql -u root -p
```



![image-20191215214412769](C:\Users\xl521\AppData\Roaming\Typora\typora-user-images\image-20191215214412769.png)

⑩设置开启启动

```shell
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld
chkconfig --add mysqld
```





## 3.安装完成

这样一个初步的mysql的离线安装就完成了

如果觉得离线安装比较麻烦，也可以使用unbuntu的apt-get或者CentOS的yum在线安装mysql