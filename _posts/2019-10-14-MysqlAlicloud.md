---
dlayout:     post
title:      Fail to Enter AliCloud MySQL
subtitle:   远程连接阿里云MySQL失败解决办法
date:       2019-10-14
author:     Kylin
header-img: img/bg-mysql.jpg
catalog: true
tags:
    - Cloud

---



# Fail to Enter AliCloud MySQL



- 服务端Mysql安装

先保证在服务端安装mysql，所以更新包索引

```
sudo apt update
```

执行安装

```
sudo apt install mysql-server
```

安装结束后，打开Mysql服务

```
sevice mysql start
```

服务端登陆数据库，查验是否安装成功

```
mysql -uroot -p
```



- 修改MySQL监听IP

默认情况下MySQL监听的是127.0.0.1，也就是本机，所以只有本机能够连接上，因此需要将MySQL改成监听远程主机IP或者所有IP。 
以Ubuntu为例，打开修改以下文件：

```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

找到bind-address，如果监听固定远程IP，则改成远程主机IP，若监听所有IP，则改成0.0.0.0或者注释bind-address。修改完成后重启MySQL：

```
service mysql restart
```

Tips：根据设备类型设置3306端口开放，如阿里云应在控制台防火墙页面开放TCP/IP 3306端口。



- 客户端登陆

客户端通过命令行连接

```
mysql -u root -h YourIP -p
```



- 服务端查看监听

```
netstat -ano | grep 3306
```

此时应该可以看到客户端IP在连接列表中