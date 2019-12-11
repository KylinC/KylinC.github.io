---
dlayout:     post
title:      Enter with Root in Tencent ECS
subtitle:   配置Root模式登陆腾讯云服务器
date:       2019-12-11
author:     Kylin
header-img: img/Tcloud.jpg
catalog: true
tags:
    - Cloud
    - TencentCloud

---



# Enter with Root in Tencent ECS



> 在腾讯云服务器上自动分配的登陆用户名是ubuntu，如果需要root登陆，需要在服务器端进行简单配置



### ssh登陆用户'ubuntu'

```bash
ssh ubuntu@yourip
```



### 设置root密码

```bash
sudo passwd root
```



### 修改ssh权限

```bash
sudo vim /etc/ssh/sshd_config
```



### 修改PermitRootLogin

找到PermitRootLogin一项，取消注释并且将**prohibit-password**改为**yes**

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-11-%E6%88%AA%E5%B1%8F2019-12-11%E4%B8%8A%E5%8D%8810.53.56.png)



### 配置生效

```bash
sudo service ssh restart
```



### 重新以Root登陆即可

```bash
exit
```
