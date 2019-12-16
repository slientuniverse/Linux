[TOC]

# Linux安装jdk

[本文csdn连接](https://blog.csdn.net/sinat_32602455/article/details/93884099)

## 1.官网下载jdk1.8

> 官方地址：<https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>
>
> ![1561619655520](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561619655520.png)
>
> [^footnote]：现在下载jdk需要登录oracle，https://blog.csdn.net/Dreamer_good/article/details/89517710  这个地址里有别人分享的账号



## 2.上传jdk到安装目录

> [^footnote]：使用rz命令上传安装包，rz命令安装教程：<https://blog.csdn.net/sinat_32602455/article/details/93738887>
>
> ![1561620475774](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561620475774.png)



## 3.安装jdk

> ①解压文件：
>
> tar -xzvf jdk-8u212-linux-x64.tar.gz
>
> ②配置环境变量：
>
> 输入 vi /etc/profile
>
> ![1561621112613](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561621112613.png)
>
> 输入i进入编辑模式
>
> 在配置文件的最下方添加
>
> export JAVA_HOME=/usr/local/java/jdk1.8.0_212
> export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
> export PATH=$PATH:$JAVA_HOME/bin
>
> 添加完成按ESC退出编辑模式，输入:wq保存文件
>
> ![1561620932479](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561620932479.png)
>
> 保存输入 source /etc/profile 来生效更改的配置
>
> 输入java -version查看是否安装成功
>
> ![1561621283197](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1561621283197.png)
>
> 安装成功！

