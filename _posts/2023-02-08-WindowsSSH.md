---
dlayout:    post
title:      通过SSH连接Windows
subtitle:   通过SSH远程访问Windows服务器
date:       2023-02-08
author:     Kylin
header-img: img/post-cache.jpg
catalog: true
tags:
    - Linux
---



## 通过SSH连接Windows服务器

注：以下命令需要windows管理员权限运行（真tm麻烦）

### 服务器端：安装OpenSSH

- 在设置中安装

在`设置->应用和功能->可选功能` 内安装openssh客户端和服务器 (OpenSSH client & OpenSSH server)

![截屏2023-02-08 16.51.00](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-02-08%2016.51.00.png)

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-02-08%2016.52.19.png" alt="截屏2023-02-08 16.52.19" style="zoom:50%;" />



- 通过Powershell安装

```
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```



### 服务器端：启动OpenSSH服务

- 在服务中启动

在`任务管理器->服务->打开服务` 内开启openssh相关服务（可以设置开机自启动）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2023-02-08%2016.57.10.png" alt="截屏2023-02-08 16.57.10" style="zoom:47%;" />

- Powershell方式启动

```
net start sshd
```



### 服务器端：配置OpenSSH

- 查看windows防火墙设置

```
# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

- 配置端口

打开配置文件`ProgramData\.ssh\sshd_config` 修改端口号并取消注释以下内容：

```
PasswordAuthentication yes

IgnoreUserKnownHosts no
IgnoreRhosts yes

Match Group administrators
       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```



### 服务器端：配置自动连接到PowerShell

ssh默认是连接到cmd的，这意味着很难使用类linux指令，所以我们连接ssh服务到powershell：

```
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```



### 客户端：连接

```
ssh username@ip_address -p 7070
```

username可不填，默认管理员用户；ip_address是主机所在ip地址，若在局域网内（某些学生党或是无良宽带商家），可以配置内网穿透，可以看[这篇教程](http://kylinchen.cn/2019/05/29/SSH/)。

密码是windows锁屏密码，不是microsoft账号密码。

