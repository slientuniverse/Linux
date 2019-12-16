[TOC]

# Linux安装redis

> redis现在是必不可少的数据库，很多场景都能用到，所以



## 1.安装C语言编译环境

> 安装redis需要C语言编译环境：
>
> root@JD:/usr/local/redis#sudo apt-get update
>
> root@JD:/usr/local/redis#sudo apt-get install gcc
>
> ![1561690288653](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561690288653.png)
>
> 查看gcc版本
>
> root@JD:/# gcc --version
>
> ![1561690366690](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561690366690.png)
>
> root@JD:/# sudo apt-get install make
>
> ![1561702098065](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561702098065.png)



## 2.下载redis安装包

> 官网下载地址：<https://redis.io/download>
>
> 现在redis的稳定版本已经到了5.x时代，我之前一直在用4.0.9，这台新机器准备安装5.0.5版本
>
> redis-5.0.5.tar.gz
>
> ![1561688758202](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561688758202.png)



## 3.安装redis

> ①上传安装包
>
> ![1561688843639](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561688843639.png)
>
> 
>
> ②解压安装包
>
> tar xzvf redis-5.0.5.tar.gz
>
> ![1561689266142](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561689266142.png)
>
> 
>
> ③安装redis
>
> root@JD:/usr/local/redis# cd redis-5.0.5
>
> root@JD:/usr/local/redis/redis-5.0.5# make
>
> root@JD:/usr/local/redis/redis-5.0.5# cd src/
>
> root@JD:/usr/local/redis/redis-5.0.5/src# sudo make install PREFIX=/usr/local/redis
>
> 执行完这步之后会看到在/usr/local/redis里生成了一个bin文件夹
>
> ![1561702900583](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561702900583.png)
>
> ④配置redis
>
> 复制conf文件到bin目录下
>
> root@JD:/usr/local/redis# cd redis-5.0.5
>
> root@JD:/usr/local/redis/redis-5.0.5# cp redis.conf ../bin/
>
> ![1561703005686](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561703005686.png)
>
> 编辑配置文件：
>
> root@JD:/usr/local/redis/bin# vi redis.conf
>
> 输入i进去编辑模式
>
> ![1561703143382](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561703143382.png)
>
> 设置redis为后台启动
>
> 将**daemonize no** 改成**daemonize yes**
>
> ![1561703792996](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561703792996.png)
>
> redis开启远程访问
>
> 将**bind 127.0.0.1** 注释掉
>
> ![1561703607457](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561703607457.png)
>
> 修改redis端口号
>
> port [端口号]
>
> ![1561703889900](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561703889900.png)
>
> 修改密码
>
> 讲requirepass 注释释放开，输入你要设置的密码
>
> ![1561704243806](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561704243806.png)
>
> 
>
> ⑤启动redis
>
> 进入bin目录
>
> root@JD:/usr/local/redis/bin# redis-server
>
> ![1561704449310](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561704449310.png)
>
> 
>
> 后台启动redis
>
> root@JD:/usr/local/redis/bin# redis-server redis.conf &
>
> ![1561704602285](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561704602285.png)
>
> redis启动成功！
>
> 1
>
> ⑥本地测试redis
>
> root@JD:/usr/local/redis/bin# redis-cli
>
> ![1561704722445](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561704722445.png)
>
> ⑦远程连接测试
>
> 开放了远程连接一定不能忘了设置密码哦！
>
> 暂时将密码设置为123456
>
> 测试使用redis desktop manager
>
> ![1561705198661](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561705198661.png)
>
> 验证通过
>
> ![1561705334300](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561705334300.png)
>
> 安装成功！！！
