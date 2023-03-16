---
dlayout:    post
title:      Linux基础操作
subtitle:   Linux Basics
date:       2023-3-16
author:     Kylin
header-img: img/ubuntu.jpg
catalog: true
tags:
    - Linux
---



[TOC]

## Linux学习路径

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-122326.png" alt="it SIMIEICSTM" style="zoom:45%;" />

## 简介

### Unix族谱

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-122022.png" alt="Unnamed PDP-7 operating system" style="zoom: 25%;" />



### HotKeys

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-124419.png" alt="RANDERNITA, LTEnd" style="zoom:33%;" />



### 命令行正则（ls *.txt）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-124346.png" alt="I LIE M7N" style="zoom:33%;" />



## 用户、文件权限

> Linux 的用户管理和权限机制 = 不同的用户不能轻易地查看、修改彼此的文件。



### who am i

- 查看伪终端序号

```
//终端用户
who am i
//登陆用户
whoami
# 或者

who mom likes
```

- who命令参数

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-141530.png" alt="截屏2021-09-05 下午10.14.37" style="zoom:33%;" />



### su,su- 和 sudo

- su \<user\> 可以切换到用户用户，输入时需要输入目标用户的密码
- sudo \<cmd\> 可以以特权级别运行cmd命令，需要当前用户属于sudo组，且需要输入当前用户的密码
- su - \<user \> 相应的变化用户，同时也伴随着用户的环境变量和工作目录的变化而形成目标用户所的。

```
//添加新用户lilei
sudo adduser lilei 
//登陆
su -l lilei
//登出
Ctrl+D
```



### 用户组 groups

#### 查看组

```
method 1:
// 使用groups命令，return：
// 用户名：用户组
groups shiyanlou
// 新建用户如果不指定用户组的话，默认会自动创建一个与用户名相同的用户组

method 2:
//查看group文件并且排序
cat /etc/group | sort
//查看group文件
cat /etc/group | grep -E "shiyanlou"
//group文件格式
//组名：密码(不可见显示为x)：GID：user_list
```

#### 加入sudo组

```
# 注意 Linux 上输入密码是不会显示的
groups lilei
sudo usermod -G sudo lilei
groups lilei
```



#### 在sudo组之外实现sudo

```
创建文件/etc/sudoers.d/用户名
```

文件内容：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-05-144355.png" alt="截屏2021-09-05 下午10.43.44" style="zoom: 50%;" />



### 删除用户（组）

```
sudo deluser lilei --remove-home
// --remove-home表示将工作目录一同删除

groupdel somegroup
//必须在里面的用户都删除之后才可以删组
```



### 文件权限

#### check文件权限

```
//list式显示
ls -l
//list+size具体显示
ls -lh
//显示所有（含隐藏）
ls -a
//看某目录
ls -dl <目录名>
//大小排序，其中小 s 为显示文件大小，大 S 为按文件大小排序
ls -asSh
```

>  实例：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-031950.png" alt="uid871732-20200302-1583147815919" style="zoom:67%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-032003.png" alt="3-9" style="zoom:67%;" />

> 权限注释：
>
> - 文件类型：关于文件类型，这里有一点你必需时刻牢记 Linux 里面一切皆文件，正因为这一点才有了设备文件（ /dev 目录下有各种设备文件，大都跟具体的硬件设备相关）这一说。 socket：网络套接字，具体是什么，感兴趣的用户可以去学习实验楼的后续相关课程。 pipe 管道，这个东西很重要，我们以后将会讨论到，这里你先知道有它的存在即可。软链接文件：链接文件是分为两种的，另一种当然是“硬链接”（硬链接不常用，具体内容不作为本课程讨论重点，而软链接等同于 Windows 上的快捷方式，你记住这一点就够了）。
>
> - 文件权限：读权限，表示你可以使用 cat \<file\> 之类的命令来读取某个文件的内容；写权限，表示你可以编辑和修改某个文件的内容；
>
>   执行权限，通常指可以运行的二进制程序文件或者脚本文件，如同 Windows 上的 exe 后缀的文件，不过 Linux 上不是通过文件后缀名来区分文件的类型。你需要注意的一点是，**一个目录同时具有读权限和执行权限才可以打开并查看内部文件，而一个目录要有写权限才允许在其中创建其它文件，这是因为目录文件实际保存着该目录里面的文件的列表等信息。**
>
>   所有者权限，这一点相信你应该明白了，至于所属用户组权限，是指你所在的用户组中的所有其它用户对于该文件的权限，比如，你有一个 iPad，那么这个用户组权限就决定了你的兄弟姐妹有没有权限使用它破坏它和占有它。
>
> - 

![3-10](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-032026.png)

#### 改变文件属主（chown=change owner）

```
//在sudo下进行修改: sudo chown new_owner filename
sudo chown shiyanlou iphone11
```

####改变文件权限

> 文件权限的两种表示形式

- 方式一：二进制数字表示

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-033828.png" alt="3-14" style="zoom:40%;" />

> - 每个文件有三组固定的权限，分别对应拥有者，所属用户组，其他用户，记住这个顺序是固定的。文件的读写执行对应字母 rwx，以二进制表示就是 111，用十进制表示就是 7，对进制转换不熟悉的同学可以看看 进制转换。例如我们刚刚新建的文件 iphone11 的权限是 rw-rw-rw-，换成对应的十进制表示就是 666，这就表示这个文件的拥有者，所属用户组和其他用户具有读写权限，不具有执行权限。

```
chmod 600 iphone11
```



- 方式二：加减赋值操作

```
chmod go-rw iphone11
// 语义：group、others 减去 r、w权限
```

> g、o 还有 u 分别表示 group（用户组）、others（其他用户） 和 user（用户），  和 - 分别表示增加和去掉相应的权限。



#### adduser 和 useradd 的区别是什么 (动词前置是程序)

答：useradd 只创建用户，不会创建用户密码和工作目录，创建完了需要使用 passwd \<username \> 去设置新用户的密码。 adduser 在创建用户的同时，会创建工作目录和密码（提示你设置），做这一系列的操作。其实 useradd、userdel 这类操作更像是一种命令，执行完了就返回。而 adduser 更像是一种程序，需要你输入、确定等一系列操作。



## Linux 目录结构及文件基本操作

### FHS（Filesystem Hierarchy Standard, 文件系统层次结构标准）

> 多数 Linux 版本采用这种文件组织形式，FHS 定义了系统中每个区域的用途、所需要的最小构成的文件和目录同时还给出了例外处理与矛盾处理。

FHS 定义了两层规范:

- 第一层是， / 下面的各个目录应该要放什么文件数据，例如 /etc 应该放置设置文件，/bin 与 /sbin 则应该放置可执行文件等等。

- 第二层是，针对 /usr 及 /var 这两个目录的子目录来定义。例如 /var/log 放置系统日志文件，/usr/share 放置共享数据等等。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-062824.png" alt="4-1" style="zoom:80%;" />

- FHS 依据文件系统使用的频繁与否以及是否允许用户随意改动，将目录定义为四种交互作用的形态，如下表所示：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-06-063051.png" alt="document-uid18510labid59timestamp1482919171956" style="zoom:47%;" />

```
sudo apt-get update

sudo apt-get install tree

tree /
```



### 路径

#### ~/. 和 ~ 和 ~/.. 和 -

- 在 Linux 里 . 表示当前目录，.. 表示上一级目录（注意，我们上一节介绍过的，以 . 开头的文件都是隐藏文件，所以这两个目录必然也是隐藏的，你可以使用 ls -a 命令查看隐藏文件）
- 表示上一次所在目录，～ 通常表示当前用户的 home 目录
- 因此 ~/. 等价于 ~ ，表示当前用户目录，即 /home/username
- ~/.. 为home目录，即 /home
- pwd为绝对路径

实例：以/home/usrname下为例：

```
# 绝对路径
cd /usr/local/bin
# 相对路径
cd ../../usr/local/bin
```

> 提示：在进行目录切换的过程中请多使用 Tab 键自动补全，可避免输入错误，**连续按两次 Tab 可以显示全部候选结果。**



### Linux文件操作 (新建、复制、删除、移动文件与文件重命名、查看文件、查看文件类型、以及编辑文件)



#### 新建

- 新建文件

```
touch test
//touch 命令，其主要作用是来更改已有文件的时间戳的（比如，最近访问时间，最近修改时间），但其在不加任何参数的情况下，只指定一个文件名，则可以创建一个指定文件名的空白文件；
//touch一个已经存在的文件（或文件夹），不覆盖，只更新时间戳（最近访问、修改时间）
```

- 新建文件夹

```
mkdir mydir
//存在mydir文件夹则报错
mkdir -p father/son/grandson
//使用 -p 参数，同时创建父目录（如果不存在该父目录）
```



#### 复制

- 复制文件

```
cp test father/son/grandson
// cp 文件 目标地址

cp test father/son/grandson/test
// cp 文件 目标地址(新命名文件名)
```

- 复制目录

```
cp -r father family
// cp 目录 目标地址，但是必须加-r或-R使得复制递归进行
```



#### 删除

- 删除文件

```
rm test

//force删除：权限不足也要删除
rm -f test
```

- 删除文件夹

```
rm -r family

//force删除：权限不足也要删除
rm -rf family
```



#### 文件移动（附带重命名）

- 文件移动

```
mkdir Documents
touch file1
mv file1 Documents
//命令格式：mv 源目录文件 目的目录
```

- 文件重命名

```
mv file1 myfile
```

- 批量重命名（rename命令）

```
cd /home/shiyanlou/

# 使用通配符批量创建 5 个文件:
touch file{1..5}.txt

# 批量将这 5 个后缀为 .txt 的文本文件重命名为以 .c 为后缀的文件:
rename 's/\.txt/\.c/' *.txt

# 批量将这 5 个文件，文件名和后缀改为大写:
rename 'y/a-z/A-Z/' *.c
```



#### 查看文件

##### 使用 cat，tac 和 nl 命令查看文件

- cat和tac将文件打印到标准输出（终端）

```
cat -n file
//按行打印
```

cat 为正序显示，tac 为倒序显示

> 标准输入输出：当我们执行一个 shell 命令行时通常会自动打开三个标准文件，即标准输入文件（stdin），默认对应终端的键盘、标准输出文件（stdout）和标准错误输出文件（stderr），后两个文件都对应被重定向到终端的屏幕，以便我们能直接看到输出内容。进程将从标准输入文件中得到输入数据，将正常输出数据输出到标准输出文件，而将错误信息送到标准错误文件中。

- nl

```
-b : 指定添加行号的方式，主要有两种：
    -b a:表示无论是否为空行，同样列出行号("cat -n"就是这种方式)
    -b t:只列出非空行的编号并列出（默认为这种方式）
-n : 设置行号的样式，主要有三种：
    -n ln:在行号字段最左端显示
    -n rn:在行号字段最右边显示，且不加 0
    -n rz:在行号字段最右边显示，且加 0
-w : 行号字段占用的位数(默认为 6 位)
```



##### 使用 more 和 less 命令分页查看文件

```
more passwd
```

> 可以使用 Enter 键向下滚动一行，使用 Space 键向下滚动一屏，按下 h 显示帮助，q 退出。



##### 使用 head 和 tail 命令查看文件

> 查看头几行or尾几行

```
tail /etc/passwd
//显示尾10行，默认

tail -n 1 /etc/passwd
//显示尾一行

tail -f /etc/passwd
//监听
```



##### 查看文件类型 （file命令）

```
tail -n 1 /etc/passwd
```



## 环境变量与文件查找

### 环境变量

> 即Shell 变量，所谓变量就是计算机中用于记录一个值（不一定是数值，也可以是字符或字符串）的符号，而这些符号将用于不同的运算处理中。通常变量与值是一对一的关系，可以通过表达式读取它的值并赋值给其它变量，也可以直接指定数值赋值给任意变量。为了便于运算和处理，大部分的编程语言会区分变量的类型，用于分别记录数值、字符或者字符串等等数据类型。 Shell 中的变量也基本如此，有不同类型（但不用专门指定类型名），可以参与运算，有作用域限定。
>
> (详细需参考ABS Guide)

- 变量

使用 declare 命令创建一个变量名为 tmp 的变量并赋值：

```bash
declare tmp

# 正确的赋值
tmp=shiyanlou
# 错误的赋值
tmp = shiyanlou
//使用 = 号赋值运算符，将变量 tmp 赋值为 shiyanlou。注意，与其他语言不同的是， Shell 中的赋值操作，= 两边不可以输入空格，否则会报错。

echo $tmp
//读取变量的值，使用 echo 命令和 $ 符号（$ 符号用于表示引用一个变量的值，初学者经常忘记输入）
```

- 什么是环境变量

>  关于哪些变量是环境变量，可以简单地理解成在当前进程的子进程有效则为环境变量，否则不是（有些人也将所有变量统称为环境变量，只是以全局环境变量和局部环境变量进行区分，我们只要理解它们的实质区别即可）

- 环境变量

tips：为了与普通变量区分，通常我们习惯将环境变量名设为大写。

> 环境变量的作用域比自定义变量的要大，如 Shell 的环境变量作用于自身和它的子进程。在所有的 UNIX 和类 UNIX 系统中，每个进程都有其各自的环境变量设置，且默认情况下，当一个进程被创建时，除了创建过程中明确指定的话，它将继承其父进程的绝大部分环境设置。 Shell 程序也作为一个进程运行在操作系统之上，而我们在 Shell 中运行的大部分命令都将以 Shell 的子进程的方式运行。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-011824.png" alt="5-2" style="zoom:50%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-012044.png" alt="截屏2021-09-07 上午9.20.11" style="zoom:50%;" />



Eg. vimdiff比较

```
env|sort>env.txt
export|sort>export.txt
set|sort>set.txt
```

Eg. export实例

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-013300.png" alt="document-uid735639labid60timestamp1532339293501" style="zoom:50%;" />



- 按变量的生存周期来划分，Linux 变量可分为两类：
  - 永久的：需要修改配置文件，变量永久生效；
  - 临时的：使用 export 命令行声明即可，变量在关闭 shell 时失效。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-014024.png" alt="截屏2021-09-07 上午9.40.16" style="zoom:50%;" />

#### PATH

> 环境变量 PATH 里面就保存了 Shell 中执行的命令的搜索路径。

```
echo $PATH

//通常这一类目录下放的都是可执行文件，当我们在 Shell 中执行一个命令时，系统就会按照 PATH 中设定的路径按照顺序依次到目录中去查找，如果存在同名的命令，则执行先找到的那个。
```

- Naive添加PATH路径

```
PATH=$PATH:/home/shiyanlou/mybin
//注意到 PATH 里面的路径是以 : 作为分隔符的
//注意这里一定要使用绝对路径。

//至此，mybin下的文件可以直接执行，且不用添加./
hello_world
```

> 现在你就可以在任意目录执行那两个命令了（注意需要去掉前面的 ./）。你可能会意识到这样还并没有很好的解决问题，因为我给 PATH 环境变量追加了一个路径，它也只是在当前 Shell 有效，我一旦退出终端，再打开就会发现又失效了。有没有方法让添加的环境变量全局有效？或者每次启动 Shell 时自动执行上面添加自定义路径到 PATH 的命令？下面我们就来说说后一种方式——让它自动执行。

- 配置文件添加PATH路径

查看已经安装shell

```
cat /etc/shells
```

追加shell

```
echo "PATH=$PATH:/home/shiyanlou/mybin" >> .zshrc

//上述命令中 >> 表示将标准输出以追加的方式重定向到一个文件中，注意前面用到的 >;\ 是以覆盖的方式重定向到一个文件中，使用的时候一定要注意分辨。在指定文件不存在的情况下都会创建新的文件。

source .zshrc
//使环境变量立即生效
```



#### 环境变量修改、删除

- 修改

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-023128.png" alt="截屏2021-09-07 上午10.31.18" style="zoom:40%;" />

Eg.

```
mypath=$PATH
echo $mypath
mypath=${mypath%/home/shiyanlou/mybin}
# 或使用通配符 * 表示任意多个任意字符
mypath=${mypath%*/mybin}
```

​	

- 删除

```
unset mypath
```



#### 环境变量立即生效

> 在 Shell 中修改了一个配置脚本文件之后（比如 zsh 的配置文件 home 目录下的 .zshrc），每次都要退出终端重新打开甚至重启主机之后其才能生效。

可以使用 source 命令来让其立即生效：

```
source .zshrc

. ./.zshrc
//source 命令还有一个别名就是 . 
//注意第一个点后面有一个空格，而且后面的文件必须指定完整的绝对或相对路径名，source 则不需要。
```



### 文件查找（whereis,which,find 和 located）

#### whereis ：简单快速

> 搜索很快，因为它并没有从硬盘中依次查找，而是直接从数据库中查询
>
> **whereis 只能搜索二进制文件（-b），man 帮助文件（-m）和源代码文件（-s）。如果想要获得更全面的搜索结果可以使用 locate 命令。**

Eg.

```
whereis who
//查找文件名含who的文件

whereis find
//查找文件名含find的文件
```

#### locate ：快而全

> 使用 locate 命令查找文件也不会遍历硬盘，它通过查询 /var/lib/mlocate/mlocate.db 数据库来检索信息。
>
> 它可以用来查找指定目录下的不同文件类型。

数据库也不是实时更新的，系统会使用定时任务每天自动执行 updatedb 命令来更新数据库。所以有时候你刚添加的文件，它可能会找不到，需要手动执行一次 updatedb 命令（在我们的环境中必须先执行一次该命令）。注意这个命令也不是内置的命令，在部分环境中需要手动安装，然后执行更新。

```
sudo apt-get update
sudo apt-get install locate
sudo updatedb
```

Eg.

```
locate /etc/sh
//查找 /etc 下所有以 sh 开头的文件
//注意，它不只是在 /etc 目录下查找，还会自动递归子目录进行查找。

locate /usr/share/*.jpg
//查找 /usr/share/ 下所有 jpg 文件
//环境里使用 zsh，在 ~/.zshrc 文件里添加了 setopt nonomatch 配置，这样就不会自动处理和修复命令，因此可以不使用 \ 转义。如果其他环境中执行该命令提示 zsh: no matches found: /usr/share/*.jpg，则可以在 .zshrc 中添加上述配置，或者使用 \ 转义。
```

如果想只统计数目可以加上 -c 参数，-i 参数可以忽略大小写进行查找，whereis 的 -b、-m、-s 同样可以使用。

#### which ：小而精

> which 本身是 Shell 内建的一个命令，我们通常使用 which 来确定是否安装了某个指定的程序，因为它只从 PATH 环境变量指定的路径中去搜索命令并且返回第一个搜索到的结果。也就是说，我们可以看到某个系统命令是否存在以及执行的到底是哪一个地方的命令。

```
which man
which nginx
which ping
```

####find ：精而细

> find 应该是这几个命令中最强大的了，它不但可以通过文件类型、文件名进行查找而且可以根据文件的属性（如文件的时间戳，文件的权限等）进行搜索。

命令格式：find \[path\]\[option\] \[action\]

```
sudo find /etc/ -name interfaces
//这条命令表示去 /etc/ 目录下面 ，搜索名字叫做 interfaces 的文件或者目录。这是 find 命令最常见的格式，千万记住 find 的第一个参数是要搜索的地方。命令前面加上 sudo 是因为 shiyanlou 只是普通用户，对 /etc 目录下的很多文件都没有访问的权限，如果是 root 用户则不用使用。
```

- time限定

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-030551.png" alt="截屏2021-09-07 上午11.05.45" style="zoom:33%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-07-030714.png" alt="截屏2021-09-07 上午11.07.00" style="zoom:50%;" />

```
find ~ -mtime 0
//列出 home 目录中，当天（24 小时之内）有改动的文件

find ~ -newer /etc
//列出用户家目录下比 /etc 目录新的文件
```

#### grep查找

> 查找/etc/下文件名含有cron的文件

```
ll /etc/ |grep cron

#or

ls -l /etc/ |grep cron
```



## 文件打包&解压缩

- 打包&压缩格式

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-21-063523.png" alt="截屏2021-09-21 下午2.34.36" style="zoom:40%;" />

### zip压缩打包

```bash
cd /home/shiyanlou
zip -r -q -o shiyanlou.zip /home/shiyanlou/Desktop
du -h shiyanlou.zip
file shiyanlou.zip
```

> 上面命令将目录 /home/shiyanlou/Desktop 打包成一个文件，并查看了打包后文件的大小和类型。第一行命令中，-r 参数表示递归打包包含子目录的全部内容，-q 参数表示为安静模式，即不向屏幕输出信息，-o，表示输出文件，需在其后紧跟打包输出文件名。

```bash
zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou/Desktop -x ~/*.zip
zip -r -1 -q -o shiyanlou_1.zip /home/shiyanlou/Desktop -x ~/*.zip

// 设置压缩级别为 9 和 1（9 最大，1 最小），重新打包
// 这里添加了一个参数用于设置压缩级别 -[1-9]，1 表示最快压缩但体积大，9 表示体积最小但耗时最久。最后那个 -x 是为了排除我们上一次创建的 zip 文件，否则又会被打包进这一次的压缩文件中，注意：这里只能使用绝对路径，否则不起作用。

du -h -d 0 *.zip ~ | sort
```

- 加密zip包

```bash
zip -r -e -o shiyanlou_encryption.zip /home/shiyanlou/Desktop
```

- Linux向Windows兼容

> 关于 zip 命令，因为 Windows 系统与 Linux/Unix 在文本文件格式上的一些兼容问题，比如换行符（为不可见字符），在 Windows 为 CR LF（Carriage-Return Line-Feed：回车加换行），而在 Linux/Unix 上为 LF（换行），所以如果在不加处理的情况下，在 Linux 上编辑的文本，在 Windows 系统上打开可能看起来是没有换行的。如果你想让你在 Linux 创建的 zip 压缩文件在 Windows 上解压后没有任何问题，那么你还需要对命令做一些修改：

```bash
zip -r -l -o shiyanlou.zip /home/shiyanlou/Desktop

// 需要加上 -l 参数将 ELF 转换为 CR LF 来达到以上目的。
```



### unzip解压缩zip文件

```bash
unzip shiyanlou.zip

// 安静模式
unzip -q shiyanlou.zip -d ziptest

// 仅查看（不解压缩）
unzip -l shiyanlou.zip
```

> 使用 unzip 解压文件时我们同样应该注意兼容问题，不过这里我们关心的不再是上面的问题，而是中文编码的问题，通常 Windows 系统上面创建的压缩文件，如果有有包含中文的文档或以中文作为文件名的文件时默认会采用 GBK 或其它编码，而 Linux 上面默认使用的是 UTF-8 编码，如果不加任何处理，直接解压的话可能会出现中文乱码的问题（有时候它会自动帮你处理），为了解决这个问题，我们可以在解压时指定编码类型。

```bash
unzip -O GBK 中文压缩文件.zip
// 使用 -O（英文字母，大写 o）参数指定编码类型
```



### tar打包

#### tar打包（创建归档文件）和解包

- 创建一个 tar 包：

```bash
cd /home/shiyanlou
tar -P -cf shiyanlou.tar /home/shiyanlou/Desktop

// 上面命令中，-P 保留绝对路径符，-c 表示创建一个 tar 包文件，-f 用于指定创建的文件名，注意文件名必须紧跟在 -f 参数之后，比如不能写成 tar -fc shiyanlou.tar，可以写成 tar -f shiyanlou.tar -c ~。你还可以加上 -v 参数以可视的的方式输出打包的文件。
```

- 解包一个文件（-x 参数）到指定路径的已存在目录（-C 参数）：

```
mkdir tardir
tar -xf shiyanlou.tar -C tardir
```

- 只查看（不解包）

```
tar -tf shiyanlou.tar
```

- 保留文件属性和跟随链接（符号链接或软链接），有时候我们使用 tar 备份文件当你在其他主机还原时希望保留文件的属性（-p 参数）和备份链接指向的源文件而不是链接本身（-h 参数）：

```
tar -cphf etc.tar /etc
```

#### 使用 gzip 工具创建 *.tar.gz 文件

- 我们只需要在创建 tar 文件的基础上添加 -z 参数，使用 gzip 来压缩文件：

```
tar -czf shiyanlou.tar.gz /home/shiyanlou/Desktop
```

- 解压 *.tar.gz 文件：

```
tar -xzf shiyanlou.tar.gz
```

- 其他压缩方式

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-21-072745.png" alt="截屏2021-09-21 下午3.27.31" style="zoom:33%;" />





## 文件系统操作与磁盘管理

### 基本磁盘操作命令

#### 使用 df 命令查看磁盘的容量

```
df

# 可读性方式
df -h
```

#### 使用 du 命令查看目录的容量

```
# 默认同样以块的大小展示
du
# 加上 `-h` 参数，以更易读的方式展示
du -h

# 只查看 1 级目录的信息
du -h -d 0 ~
# 查看 2 级
du -h -d 1 ~

du -h # 同 --human-readable 以 K，M，G 为单位，提高信息的可读性。
du -a # 同 --all 显示目录中所有文件的大小。
du -s # 同 --summarize 仅显示总计，只列出最后加总的值。
```

####使用 dd 命令创建虚拟镜像文件

##### dd命令

>  dd 命令用于转换和复制文件，不过它的复制不同于 cp。之前提到过关于 Linux 的很重要的一点，一切即文件，在 Linux 上，硬件的设备驱动（如硬盘）和特殊设备文件（如 /dev/zero 和 /dev/random）都像普通文件一样，只是在各自的驱动程序中实现了对应的功能，dd 也可以读取文件或写入这些文件。这样，dd 也可以用在备份硬件的引导扇区、获取一定数量的随机数据或者空数据等任务中。 dd 程序也可以在复制时处理数据，例如转换字节序、或在 ASCII 与 EBCDIC 编码间互换。dd 的命令行语句与其他的 Linux 程序不同，因为它的命令行选项格式为 选项=值，而不是更标准的 --选项 值 或 -选项=值。 dd 默认从标准输入中读取，并写入到标准输出中，但可以用选项 if（input file，输入文件）和 of（output file，输出文件）改变。

- 用 dd 命令从标准输入读入用户的输入到标准输出或者一个文件中：

```
# 输出到文件
dd of=test bs=10 count=1 # 或者 dd if=/dev/stdin of=test bs=10 count=1
# 输出到标准输出
dd if=/dev/stdin of=/dev/stdout bs=10 count=1
# 在打完了这个命令后，继续在终端打字，作为你的输入
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-21-080800.gif" alt="document-uid735639labid62timestamp1532339776332.png" style="zoom:53%;" />

> 上述命令从标准输入设备读入用户输入（缺省值，所以可省略）然后输出到 test 文件，bs（block size）用于指定块大小（缺省单位为 Byte，也可为其指定如 K，M，G 等单位），count 用于指定块数量。如上图所示，我指定只读取总共 10 个字节的数据，当我输入了 hello shiyanlou 之后加上空格回车总共 16 个字节（一个英文字符占一个字节）内容，显然超过了设定大小。使用 du 和 cat 10 个字节（那个黑底百分号表示这里没有换行符），而其他的多余输入将被截取并保留在标准输入。

- dd 在拷贝的同时实现数据转换：将输出的英文字符转换为大写再写入文件

```
dd if=/dev/stdin of=test bs=10 count=1 conv=ucase
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-21-081025.gif" alt="document-uid735639labid62timestamp1532339755321.png" style="zoom:53%;" />

##### 使用 dd 命令创建虚拟镜像文件

- 从 /dev/zero 设备创建一个容量为 256M 的空文件

```
dd if=/dev/zero of=virtual.img bs=1M count=256
du -h virtual.img
```



#### 使用 mkfs 命令格式化磁盘（我们这里是自己创建的虚拟磁盘镜像）

```
sudo mkfs.ext4 virtual.img
```



#### 使用 mount 命令挂载磁盘到目录树

> Linux/UNIX 命令行的 mount 指令是告诉操作系统，对应的文件系统已经准备好，可以使用了，而该文件系统会对应到一个特定的点（称为挂载点）。挂载好的文件、目录、设备以及特殊文件即可提供用户使用。

```bash
sudo mount
// 查看下主机已经挂载的文件系统

mount [options] [source] [directory]
mount [-o [操作选项]] [-t 文件系统类型] [-w|--rw|--ro] [文件系统源] [挂载点]
```



- 现在直接来挂载我们创建的虚拟磁盘镜像到 /mnt 目录：

```
mount -o loop -t ext4 virtual.img /mnt
# 也可以省略挂载类型，很多时候 mount 会自动识别

# 以只读方式挂载
mount -o loop --ro virtual.img /mnt
# 或者 mount -o loop,ro virtual.img /mnt

# 命令格式 sudo umount 已挂载设备名或者挂载点，如：
sudo umount /mnt
```

> 在类 UNIX 系统中，/dev/loop（或称 vnd （vnode disk）、lofi（循环文件接口））是一种伪设备，这种设备使得文件可以如同块设备一般被访问。
>
> 在使用之前，循环设备必须与现存文件系统上的文件相关联。这种关联将提供给用户一个应用程序接口，接口将允许文件视为块特殊文件（参见设备文件系统）使用。因此，如果文件中包含一个完整的文件系统，那么这个文件就能如同磁盘设备一般被挂载。
>
> 这种设备文件经常被用于光盘或是磁盘镜像。通过循环挂载来挂载包含文件系统的文件，便使处在这个文件系统中的文件得以被访问。这些文件将出现在挂载点目录。如果挂载目录中本身有文件，这些文件在挂载后将被禁止使用。

#### 使用 fdisk 为磁盘分区

```
# 查看硬盘分区表信息
sudo fdisk -l

# 进入磁盘分区模式
sudo fdisk virtual.img
```



#### 使用 losetup 命令建立镜像与回环设备的关联

```
sudo losetup /dev/loop0 virtual.img
# 如果提示设备忙你也可以使用其它的回环设备，"ls /dev/loop*"参看所有回环设备

# 解除设备关联
sudo losetup -d /dev/loop0

sudo apt-get install kpartx
sudo kpartx -av /dev/loop0

# 取消映射
sudo kpartx -dv /dev/loop0

sudo mkfs.ext4 -q /dev/mapper/loop0p1
sudo mkfs.ext4 -q /dev/mapper/loop0p5
sudo mkfs.ext4 -q /dev/mapper/loop0p6
```



## 例行性工作排程 (crontab)

> crontab 命令常见于 Unix 和类 Unix 的操作系统之中（Linux 就属于类 Unix 操作系统），用于设置周期性被执行的指令。
>
> crontab 命令从输入设备读取指令，并将其存放于 crontab 文件中，以供之后读取和执行。通常，crontab 储存的指令被守护进程激活，crond 为其守护进程，crond 常常在后台运行，每一分钟会检查一次是否有预定的作业需要执行。

### crontab格式：

```
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

### crontab准备

- rsyslog：以便了解我们的任务是否真正的被执行

```
sudo apt-get install -y rsyslog
sudo service rsyslog start
```

- crontab

```
sudo cron －f &
```

### crontab使用

```
crontab -e
//添加一个计划任务
```

#### 在文档的最后一排加上命令

```
*/1 * * * * touch /home/shiyanlou/$(date +\%Y\%m\%d\%H\%M\%S)

// 该任务是每分钟我们会在/home/shiyanlou 目录下创建一个以当前的年月日时分秒为名字的空白文件
```

> “ % ” 在 crontab 文件中，有结束命令行、换行、重定向的作用，前面加 ” \ ” 符号转义，否则，“ % ” 符号将执行其结束命令行或者换行的作用，并且其后的内容会被做为标准输入发送给前面的命令。

#### 查看当前执行命令

```
crontab -l
```

#### cron进程启动

```
ps aux | grep cron

# or

pgrep cron
```

#### 查看日志

```
sudo tail -f /var/log/syslog
```

#### 删除任务

```
crontab -r
```

### cron文件结构

- 每个用户使用 crontab -e 添加计划任务，都会在 /var/spool/cron/crontabs 中添加一个该用户自己的任务文档，这样目的是为了隔离。

- 系统级别（sudo）的crontab直接编辑 /etc/crontab

```
sudo crontab -e 表示为root用户添加计划任务
```

- cron 服务监测时间最小单位是分钟，所以 cron 会每分钟去读取一次 /etc/crontab 与 /var/spool/cron/crontabs 里面的内容。
- 在 /etc 目录下，cron 相关的目录有下面几个：
  - /etc/cron.daily，目录下的脚本会每天执行一次，在每天的 6 点 25 分时运行；
  - /etc/cron.hourly，目录下的脚本会每个小时执行一次，在每小时的 17 分钟时运行；
  - /etc/cron.monthly，目录下的脚本会每月执行一次，在每月 1 号的 6 点 52 分时运行；
  - /etc/cron.weekly，目录下的脚本会每周执行一次，在每周第七天的 6 点 47 分时运行；

### 挑战：备份日志

- 为 shiyanlou 用户添加计划任务：
- 每天凌晨 3 点的时候定时备份 /var/log/alternatives.log 到 /home/shiyanlou/tmp/ 目录
  命名格式为 年-月-日，比如今天是 2017 年 4 月 1 日，那么文件名为 2017-04-01

```
sudo cron -f &
crontab -e # 添加
0 3 * * * sudo rm /home/shiyanlou/tmp/*
0 3 * * * sudo cp /var/log/alternatives.log /home/shiyanlou/tmp/$(date +%Y-%m-%d)
```



## 命令执行顺序控制与管道

### 命令执行顺序

#### 顺序执行（；分隔）

```
sudo apt-get update;sudo apt-get install some-tool;some-tool # 让它自己运行
```

#### 有选择执行

- &&

> 比如我们使用 which 来查找是否安装某个命令，如果找到就执行该命令，否则什么也不做，虽然这个操作没有什么实际意义

```
which cowsay>/dev/null && cowsay -f head-in ohch~

//上面的 && 就是用来实现选择性执行的，它表示如果前面的命令执行结果（不是表示终端输出的内容，而是表示命令执行状态的结果）返回 0 则执行后面的，否则不执行，你可以从 echo $? 环境变量获取上一次命令的返回结果
```

- ||

```
which cowsay>/dev/null || echo "cowsay has not been install, please run 'sudo apt-get install cowsay' to install"

//|| 在这里就是与 && 相反的控制效果，当上一条命令执行结果为 ≠0(\$?≠0) 时则执行它后面的命令：
```

- 联合使用

```
which cowsay>/dev/null && echo "exist" || echo "not exist"
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-082107.png" alt="8-3" style="zoom: 70%;" />

### 管道( | 匿名管道)

> 管道是一种通信机制，通常用于进程间的通信（也可通过 socket 进行网络通信），它表现出来的形式就是将前面每一个进程的输出（stdout）直接作为下一个进程的输入（stdin）。

- 使用less显示ls内容

```
ls -al /etc | less
```

#### cut（打印每一行的某一字段）

```
cut /etc/passwd -d ':' -f 1,6
# 按":"分隔之后取1、6段

# 前五个（包含第五个）
cut /etc/passwd -c -5
# 前五个之后的（包含第五个）
cut /etc/passwd -c 5-
# 第五个
cut /etc/passwd -c 5
# 2 到 5 之间的（包含第五个）
cut /etc/passwd -c 2-5
```

#### grep（在文本或stdin中查找）

- 命令格式：

```
grep [命令选项]... 用于匹配的表达式 [文件]...
```

- eg. 搜索/home/shiyanlou目录下所有包含"shiyanlou"的文本文件，并显示出现在文本中的行号：

```
grep -rnI "shiyanlou" ~

# -r 参数表示递归搜索子目录中的文件，-n 表示打印匹配项行号，-I 表示忽略二进制文件。这个操作实际没有多大意义，但可以感受到 grep 命令的强大与实用。
```

- 正则表达式：

```
# 查看环境变量中以 "yanlou" 结尾的字符串
export | grep ".*yanlou$"
```

#### wc（统计并输出一个文件中行、单词和字节的数目）

- 统计

```
wc /etc/passwd

# 行、单词数、字节数
```

- 分别输出

```
# 行数
wc -l /etc/passwd
# 单词数
wc -w /etc/passwd
# 字节数
wc -c /etc/passwd
# 字符数
wc -m /etc/passwd
# 最长行字节数
wc -L /etc/passwd
```

#### 统计目录数

```
ls -dl /etc/*/ | wc -l
```

#### sort（排序命令）

> 将数据按照一定方式排序,然后再输出,它支持的排序有按字典排序,数字排序,按月份排序,随机排序,反转排序,指定特定字段进行排序等等。

```
# 默认为字典排序
cat /etc/passwd | sort
# 反转排序
cat /etc/passwd | sort -r
# 制定字段排序，-t参数用于指定字段的分隔符，这里是以":"作为分隔符；-k 字段号用于指定对哪一个字段进行排序
cat /etc/passwd | sort -t':' -k 3
# 以上命令改为数字排序
cat /etc/passwd | sort -t':' -k 3 -n
```

#### uniq（用于过滤或者输出重复行，只）

##### 过滤重复行

- 我们可以使用 history 命令查看最近执行过的命令（实际为读取 ${SHELL}_history 文件，如我们环境中的 .zsh_history 文件）

去掉命令后的参数并去掉重复命令：

```
history | cut -c 8- | cut -d ' ' -f 1 | uniq

# 前8个是history的命令标号
# 但是uniq只能去掉相邻的重复
history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq
# 或者
history | cut -c 8- | cut -d ' ' -f 1 | sort -u
```

##### 输出重复行

```
# 输出重复过的行（重复的只输出一个）及重复次数
history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq -dc
# 输出所有重复的行
history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq -D
```



## 文本处理

### tr命令（删除一段文本信息中的某些文字。或者将其进行转换。）

```
tr [option]...SET1 [SET2]
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-115706.png" alt="截屏2021-09-29 下午7.56.22" style="zoom:50%;" />

```
# 删除 "hello shiyanlou" 中所有的'o'，'l'，'h'
$ echo 'hello shiyanlou' | tr -d 'olh'
# 将"hello" 中的ll，去重为一个l
$ echo 'hello' | tr -s 'l'
# 将输入文本，全部转换为大写或小写输出
$ echo 'input some text here' | tr '[:lower:]' '[:upper:]'
# 上面的'[:lower:]' '[:upper:]'你也可以简单的写作'[a-z]' '[A-Z]'，当然反过来将大写变小写也是可以的
```



### col命令（将Tab换成对等数量的空格键，或反转这个操作。）

```
col [option]
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-121743.png" alt="截屏2021-09-29 下午8.17.35" style="zoom:50%;" />

```
# 查看 /etc/protocols 中的不可见字符，可以看到很多 ^I ，这其实就是 Tab 转义成可见字符的符号
cat -A /etc/protocols
# 使用 col -x 将 /etc/protocols 中的 Tab 转换为空格，然后再使用 cat 查看，你发现 ^I 不见了
cat /etc/protocols | col -x | cat -A
```



### join命令（将两个文件中包含相同内容的那一行合并在一起。）

```
join [option]... file1 file2
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-122211.png" alt="截屏2021-09-29 下午8.21.49" style="zoom:50%;" />

```
cd /home/shiyanlou
# 创建两个文件
echo '1 hello' > file1
echo '1 shiyanlou' > file2
join file1 file2
# 将 /etc/passwd 与 /etc/shadow 两个文件合并，指定以':'作为分隔符
sudo join -t':' /etc/passwd /etc/shadow
# 将 /etc/passwd 与 /etc/group 两个文件合并，指定以':'作为分隔符，分别比对第4和第3个字段
sudo join -t':' -1 4 /etc/passwd -2 3 /etc/group
```



### paste 命令（不对比数据，简单地将多个文件合并一起，以Tab隔开）

```
paste [option] file...
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-125325.png" alt="截屏2021-09-29 下午8.53.16" style="zoom:50%;" />

```
echo hello > file1
echo shiyanlou > file2
echo www.shiyanlou.com > file3
paste -d ':' file1 file2 file3
paste -s file1 file2 file3
```



## 数据流重定向

### 重定向

```
echo 'hello shiyanlou' > redirect
# 覆盖式输出到文件
echo 'www.shiyanlou.com' >> redirect
# 追加式输出到文件
cat redirect
```

### 重定向概念

Linux 默认提供了三个特殊设备，用于终端的显示和输出：

-  stdin（标准输入，对应于你在终端的输入）
-  stdout（标准输出，对应于终端的输出）
-  stderr（标准错误输出，对应于终端的输出）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-09-29-132233.png" alt="截屏2021-09-29 下午9.22.26" style="zoom:50%;" />

> 文件描述符：文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往只适用于 UNIX、Linux 这样的操作系统。

- stdin stdout

```bash
cat # 按 Ctrl+C 退出
```

- stdin file

```bash
mkdir Documents
cat > Documents/test.c <<EOF
#include <stdio.h>

int main()
{
    printf("hello world\n");
    return 0;
}

EOF
```

- file stdout

```bash
cat Documents/test.c
```

- return stdout

```
echo 'hi' | cat
```

- return file

```
echo 'hello shiyanlou' > redirect
cat redirect
```

### 标准错误重定向

> 标准输出和标准错误都被指向伪终端的屏幕显示

```
# 将标准错误重定向到标准输出，再将标准输出重定向到文件，注意要将重定向到文件写到前面
cat Documents/test.c hello.c >somefile  2>&1
# 或者只用bash提供的特殊的重定向符号"&"将标准错误和标准输出同时重定向到文件
cat Documents/test.c hello.c &>somefilehell
```

### tee：多个重定向

```
echo 'hello shiyanlou' | tee hello
```

### exec：稳定重定向

```
# 先开启一个子 Shell
zsh
# 使用exec替换当前进程的重定向，将标准输出重定向到一个文件
exec 1>somefile
# 后面你执行的命令的输出都将被重定向到文件中，直到你退出当前子shell，或取消exec的重定向（后面将告诉你怎么做）
ls
exit
cat somefile
```

### xargs：去除/n

```
cut -d: -f1 < /etc/passwd | sort | xargs echo

//这个命令用于将 /etc/passwd 文件按 : 分割取第一个字段排序后，使用 echo 命令生成一个列表。
```

### 自定义文件描述符

在 Shell 中有 9 个文件描述符。上面我们使用了也是它默认提供的 0，1，2 号文件描述符。另外我们还可以使用 3-8 的文件描述符，只是它们默认没有打开而已。

#### 查看当前 Shell 进程中打开的文件描述符

```
cd /dev/fd/;ls -Al
```

#### 自定义文件描述符

```
zsh
exec 3>somefile
# 先进入目录，再查看，否则你可能不能得到正确的结果，然后再回到上一次的目录
cd /dev/fd/;ls -Al;cd -
# 注意下面的命令>与&之间不应该有空格，如果有空格则会出错
echo "this is test" >&3
cat somefile
exit
```

#### 关闭文件描述符

```
exec 3>&-
cd /dev/fd;ls -Al;cd -
```

### 屏蔽命令输出

```
cat Documents/test.c 1>/dev/null 2>&1

exec 1>/dev/null 2>&1
```



## 正则表达式

> 正则表达式，又称正规表示式、正规表示法、正规表达式、规则表达式、常规表示法（英语：Regular Expression，在代码中常简写为 regex、regexp 或 RE），计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。

### 基本语法

#### 选择

- | 竖直分隔符表示选择，例如 boy|girl 可以匹配 boy 或者 girl。

#### 数量限定

- +表示前面的字符必须出现至少一次(1 次或多次)，例如 goo gle 可以匹配 gooogle，goooogle 等；
- ? 表示前面的字符最多出现一次（0 次或 1 次），例如，colou?r，可以匹配 color 或者 colour;

* 星号代表前面的字符可以不出现，也可以出现一次或者多次（0 次、或 1 次、或多次），例如，0*42 可以匹配 42、042、0042、00042 等。

#### 范围和优先级

- () 圆括号可以用来定义模式字符串的范围和优先级，这可以简单的理解为是否将括号内的模式串作为一个整体。例如，gr(a|e)y 等价于 gray|grey，（这里体现了优先级，竖直分隔符用于选择 a 或者 e 而不是 gra 和 ey），(grand)?father 匹配 father 和 grandfather（这里体现了范围，? 将圆括号内容作为一个整体匹配）。

#### PCRE（Perl Compatible Regular Expressions 中文含义：perl 语言兼容正则表达式，适用于perl、python、grep、egrep）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145335.png" alt="截屏2021-10-07 下午10.53.15" style="zoom:30%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145435.png" alt="截屏2021-10-07 下午10.54.05" style="zoom:30%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145459.png" alt="截屏2021-10-07 下午10.54.16" style="zoom:30%;" />

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145529.png" alt="截屏2021-10-07 下午10.54.26" style="zoom:30%;" />

#### 优先级

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145618.png" alt="截屏2021-10-07 下午10.56.11" style="zoom:30%;" />

#### regex思维导图

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-145658.png" alt="RegularExpression-1" style="zoom:50%;" />

### grep模式匹配命令

#### 表达式引擎

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-150109.png" alt="截屏2021-10-07 下午11.01.02" style="zoom:40%;" />

#### 常用参数表

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-07-150331.png" alt="截屏2021-10-07 下午11.03.18" style="zoom:30%;" />

#### 使用基本正则表达式BRE

- 位置

```
// 查找 /etc/group 文件中以 shiyanlou 为开头的行
grep '^shiyanlou' /etc/group
```

- 数量

```
# 将匹配以'z'开头以'o'结尾的所有字符串
echo 'zero\nzo\nzoo' | grep 'z.*o'
# 将匹配以'z'开头以'o'结尾，中间包含一个任意字符的字符串
echo 'zero\nzo\nzoo' | grep 'z.o'
# 将匹配以'z'开头，以任意多个'o'结尾的字符串
echo 'zero\nzo\nzoo' | grep 'zo*'
```

- 选择

```
# grep默认是区分大小写的，这里将匹配所有的小写字母
echo '1234\nabcd' | grep '[a-z]'
# 将匹配所有的数字
echo '1234\nabcd' | grep '[0-9]'
# 将匹配所有的数字
echo '1234\nabcd' | grep '[[:digit:]]'
# 将匹配所有的小写字母
echo '1234\nabcd' | grep '[[:lower:]]'
# 将匹配所有的大写字母
echo '1234\nabcd' | grep '[[:upper:]]'
# 将匹配所有的字母和数字，包括0-9，a-z，A-Z
echo '1234\nabcd' | grep '[[:alnum:]]'
# 将匹配所有的字母
echo '1234\nabcd' | grep '[[:alpha:]]'
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-073334.png" alt="截屏2021-10-10 下午3.33.21" style="zoom:30%;" />

- 排除

```
# 排除字符
echo 'geek\ngood' | grep '[^o]'
```

#### 使用拓展正则表达式ERE

> 要通过 grep 使用扩展正则表达式需要加上 -E 参数，或使用 egrep。

- 数量

```
# 只匹配"zo"
echo 'zero\nzo\nzoo' | grep -E 'zo{1}'
# 匹配以"zo"开头的所有单词
echo 'zero\nzo\nzoo' | grep -E 'zo{1,}'
```

- 选择

```
# 匹配"www.shiyanlou.com"和"www.google.com"
echo 'www.shiyanlou.com\nwww.baidu.com\nwww.google.com' | grep -E 'www\.(shiyanlou|google)\.com'
# 或者匹配不包含"baidu"的内容
echo 'www.shiyanlou.com\nwww.baidu.com\nwww.google.com' | grep -Ev 'www\.baidu\.com'
```



### sed流编辑器

> sed - stream editor for filtering and transforming text "，用于过滤和转换文本的流编辑器

#### 命令格式

```
sed [参数]... [执行命令] [输入文件]...
# 形如：
$ sed -i 's/sad/happy/' test # 表示将test文件中的"sad"替换为"happy"
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-074526.png" alt="截屏2021-10-10 下午3.45.02" style="zoom:30%;" />

#### 执行性命令

- 命令格式

```
[n1][,n2]command
[n1][~step]command
// 其中 n1,n2 表示输入内容的行号，它们之间为 , 逗号则表示从 n1 到 n2 行，如果为 ~ 波浪号则表示从 n1 开始以 step 为步进的所有行；command 为执行动作
```

- 作用范围

```
sed -i 's/sad/happy/g' test # g 表示全局范围
sed -i 's/sad/happy/4' test # 4 表示指定行中的第四个匹配字符串
```

- 一些常用动作指令：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-075035.png" alt="截屏2021-10-10 下午3.50.27" style="zoom:40%;" />

#### 实例

```
# 打印2-5行
nl passwd | sed -n '2,5p'
# 打印奇数行
nl passwd | sed -n '1~2p'
# 将输入文本中"shiyanlou" 全局替换为"hehe"，并只打印替换的那一行，注意这里不能省略最后的"p"命令
sed -n 's/shiyanlou/hehe/gp' passwd
nl passwd | grep "shiyanlou"
# 删除第30行
sed -i '30d' passwd
```



### awk文本处理语言（样式扫描和处理语言）

#### 发行版

- nawk： 在 20 世纪 80 年代中期，对 awk 语言进行了更新，并不同程度地使用一种称为 nawk(new awk) 的增强版本对其进行了替换。许多系统中仍然存在着旧的 awk 解释器，但通常将其安装为 oawk (old awk) 命令，而 nawk 解释器则安装为主要的 awk 命令，也可以使用 nawk 命令。 Dr. Kernighan 仍然在对 nawk 进行维护，与 gawk 一样，它也是开放源代码的，并且可以免费获得;
- gawk： 是 GNU Project 的 awk 解释器的开放源代码实现。尽管早期的 GAWK 发行版是旧的 AWK 的替代程序，但不断地对其进行了更新，以包含 NAWK 的特性;
- mawk 也是 awk 编程语言的一种解释器，mawk 遵循 POSIX 1003.2 （草案 11.3）定义的 AWK 语言，包含了一些没有在 AWK 手册中提到的特色，同时 mawk 提供一小部分扩展，另外据说 mawk 是实现最快的 awk。

#### 基础概念

awk 所有的操作都是基于 pattern(模式)—action(动作)对来完成的，如下面的形式：

```
pattern {action}
```

- awk将所有的动作操作用一对 {} 花括号包围起来。其中 pattern 通常是表示用于匹配输入的文本的“关系式”或“正则表达式”，action 则是表示匹配后将执行的动作。在一个完整 awk 操作中，这两者可以只有其中一个，如果没有 pattern 则默认匹配输入的全部文本，如果没有 action 则默认为打印匹配内容到屏幕。

- awk 处理文本的方式，是将文本分割成一些“字段”，然后再对这些字段进行处理，默认情况下，awk 以空格作为一个字段的分割符。

#### awk命令格式

```
awk [-F fs] [-v var=value] [-f prog-file | 'program text'] [file...]

// 其中 -F 参数用于预先指定前面提到的字段分隔符（还有其他指定字段的方式），-v 用于预先为 awk 程序指定变量，-f 参数用于指定 awk 命令要执行的程序文件，或者在不加 -f 参数的情况下直接将程序语句放在这里，最后为 awk 需要处理的文本输入，且可以同时输入多个文本文件。
```



## Linux软件安装&升级

>安装软件方式：
>
>- 在线安装
>- 从磁盘安装 deb 软件包
>- 从二进制软件包安装
>- 从源代码编译安装

### apt包管理工具

- APT 是 Advance Packaging Tool（高级包装工具）的缩写，是 Debian 及其派生发行版的软件包管理器，APT 可以自动下载，配置，安装二进制或者源代码格式的软件包，因此简化了 Unix 系统上管理软件的过程。 APT 最早被设计成 dpkg 的前端，用来处理 deb 格式的软件包。现在经过 APT-RPM 组织修改，APT 已经可以安装在支持 RPM 的系统管理 RPM 包。这个包管理器包含以 apt- 开头的多个工具，如 apt-get apt-cache apt-cdrom 等，在 Debian 系列的发行版中使用。

- 软件源镜像服务器&软件源：

  我们需要定期从服务器上下载一个软件包列表，使用 sudo apt-get update 命令来保持本地的软件包列表是最新的（有时你也需要手动执行这个操作，比如更换了软件源），而这个表里会有软件依赖信息的记录，对于软件依赖，我举个例子：我们安装 w3m 软件的时候，而这个软件需要 libgc1c2 这个软件包才能正常工作，这个时候 apt-get 在安装软件的时候会一并替我们安装了，以保证 w3m 能正常的工作。

### apt-get

#### 常用工具

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-084340.png" alt="截屏2021-10-10 下午4.42.56" style="zoom:40%;" />

#### 参数配置

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-084423.png" alt="截屏2021-10-10 下午4.44.17" style="zoom:40%;" />

#### 重新安装软件包

```
sudo apt-get --reinstall install <packagename>
```

#### 软件升级

```
# 更新软件源
sudo apt-get update

# 升级没有依赖问题的软件包
sudo apt-get upgrade

# 升级并解决依赖关系
sudo apt-get dist-upgrade
```

#### 卸载软件

```
sudo apt-get remove w3m
# 不保留配置文件的移除
sudo apt-get purge w3m
# 或者
sudo apt-get --purge remove w3m
# 移除不再需要的被依赖的软件包
sudo apt-get autoremove
```

#### 软件搜索

```
sudo apt-cache search softname1 softname2 softname3……
```



### dpkg

> dpkg (debian package) 是 Debian 软件包管理器的基础，它被伊恩·默多克创建于 1993 年。 dpkg 与 RPM 十分相似，同样被用于安装、卸载和供给和 .deb 软件包相关的信息。

网络上见到以deb形式打包的软件包，就需要使用dpkg命令来安装。

- dpkg参数表：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-085124.png" alt="截屏2021-10-10 下午4.51.09" style="zoom:40%;" />

#### 使用dpkg安装deb包

- 下载emacs的debian包：

```
sudo apt-get update
sudo apt-get -d install -y emacs
```

- 查看软件包

```
ls /var/cache/apt/archives/
```

- 安装(寻在依赖无法安装)

```
cp /var/cache/apt/archives/emacs24_24.5+1-6ubuntu1.1_amd64.deb ~
# 安装之前参看deb包的信息
sudo dpkg -I emacs24_24.5+1-6ubuntu1.1_amd64.deb
```

- 修复依赖安装

```
sudo apt-get update
sudo apt-get -f install -y
```

###从二进制文件安装

- 二进制包的安装比较简单，我们需要做的只是将从网络上下载的二进制包解压后放到合适的目录，然后将包含可执行的主程序文件的目录添加进PATH环境变量即可，如果你不知道该放到什么位置，请重新复习第四节关于 Linux 目录结构的内容。



## Linux进程管理

### 进程概念

#### 进程的衍生

- fork() 与 exec()

> fork-exec是由 Dennis M. Ritchie 创造的
>
> fork() 是一个系统调用（system call），它的主要作用就是为当前的进程创建一个新的进程，这个新的进程就是它的子进程，这个子进程除了父进程的返回值和 PID 以外其他的都一模一样，如进程的执行代码段，内存信息，文件描述，寄存器状态等等
>
> exec() 也是系统调用，作用是切换子进程中的执行程序也就是替换其从父进程复制过来的代码段与数据段

- 简单逻辑实现

```
pid_t p;

p = fork();
if (p == (pid_t) -1)
        /* ERROR */
else if (p == 0)
        /* CHILD */
else
        /* PARENT */
```

#### 僵尸进程

- 既然子进程是通过父进程而衍生出来的，那么子进程的退出与资源的回收定然与父进程有很大的相关性。当一个子进程要正常的终止运行时，或者该进程结束时它的主函数 main() 会执行 exit(n); 或者 return n，这里的返回值 n 是一个信号，系统会把这个 SIGCHLD 信号传给其父进程，当然若是异常终止也往往是因为这个信号。
- 在将要结束时的子进程代码执行部分已经结束执行了，系统的资源也基本归还给系统了，但若是其进程的进程控制块（PCB）仍驻留在内存中，而它的 PCB 还在，代表这个进程还存在（因为 PCB 就是进程存在的唯一标志，里面有 PID 等消息），并没有消亡，这样的进程称之为僵尸进程（Zombie）。
- 正常情况下，父进程会收到两个返回值：exit code（SIGCHLD 信号）与 reason for termination 。之后，父进程会使用 wait(&amp;status) 系统调用以获取子进程的退出状态，然后内核就可以从内存中释放已结束的子进程的 PCB；而如若父进程没有这么做的话，子进程的 PCB 就会一直驻留在内存中，一直留在系统中成为僵尸进程（Zombie）。
- 虽然僵尸进程是已经放弃了几乎所有内存空间，没有任何可执行代码，也不能被调度，在进程列表中保留一个位置，记载该进程的退出状态等信息供其父进程收集，从而释放它。但是 Linux 系统中能使用的 PID 是有限的，如果系统中存在有大量的僵尸进程，系统将会因为没有可用的 PID 从而导致不能产生新的进程。
- 另外如果父进程结束（非正常的结束），未能及时收回子进程，子进程仍在运行，这样的子进程称之为孤儿进程。在 Linux 系统中，孤儿进程一般会被 init 进程所“收养”，成为 init 的子进程。由 init 来做善后处理，所以它并不至于像僵尸进程那样无人问津，不管不顾，大量存在会有危害。

#### 进程0&1

- 进程 0 是系统引导时创建的一个特殊进程，也称之为内核初始化，其最后一个动作就是调用 fork() 创建出一个子进程运行 /sbin/init 可执行文件，而该进程就是 PID=1 的进程 1，而进程 0 就转为交换进程（也被称为空闲进程），进程 1 （init 进程）是第一个用户态的进程，再由它不断调用 fork() 来创建系统里其他的进程，所以它是所有进程的父进程或者祖先进程。同时它是一个守护程序，直到计算机关机才会停止。

#### 进程调用关系

```
pstree

ps －fxo user,ppid,pid,pgid,command
// 其中 pid 就是该进程的一个唯一编号，ppid 就是该进程的父进程的 pid，command 表示的是该进程通过执行什么样的命令或者脚本而产生的
```

#### 进程组 (PGID) 与 Sessions

- 每一个进程都会是一个进程组的成员，而且这个进程组是唯一存在的，他们是依靠 PGID（process group ID）来区别的，而每当一个进程被创建的时候，它便会成为其父进程所在组中的一员。

- 一般情况，进程组的 PGID 等同于进程组的第一个成员的 PID，并且这样的进程称为该进程组的领导者，也就是领导进程，进程一般通过使用 getpgrp() 系统调用来寻找其所在组的 PGID，领导进程可以先终结，此时进程组依然存在，并持有相同的 PGID，直到进程组中最后一个进程终结。

- 与进程组类似，每当一个进程被创建的时候，它便会成为其父进程所在 Session 中的一员，每一个进程组都会在一个 Session 中，并且这个 Session 是唯一存在的，

- Session 主要是针对一个 tty 建立，Session 中的每个进程都称为一个工作(job)。每个会话可以连接一个终端(control terminal)。当控制终端有输入输出时，都传递给该会话的前台进程组。 Session 意义在于将多个 jobs 囊括在一个终端，并取其中的一个 job 作为前台，来直接接收该终端的输入输出以及终端信号。其他 jobs 在后台运行。

- > 前台（foreground）就是在终端中运行，能与你有交互的

- > 后台（background）就是在终端中运行，但是你并不能与其任何的交互，也不会显示其执行的过程

#### 工作管理

- bash(Bourne-Again shell)支持工作控制（job control），而 sh（Bourne shell）并不支持。

##### 后台运行

```
ls &
//通过 & 这个符号，让我们的命令在后台中运行：
// or
ctrl+z
//放置在后台不运行

//返回 [job编号] PID
```

##### 查看后台工作

```
jobs
```

##### 后台转前台

```
# 后面不加参数提取预设工作，加参数提取指定工作的编号
# ubuntu 在 zsh 中需要 %，在 bash 中不需要 %
fg [%jobnumber]
```

##### ctrl+z后台工作

```
#与fg类似，加参则指定，不加参则取预设
bg [%jobnumber]
```

##### 删除后台工作

```
# kill的使用格式如下
kill -signal %jobnumber
kill -signal pid

# signal从1-64个信号值可以选择，可以这样查看
kill －l
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-122215.png" alt="截屏2021-10-10 下午8.22.08" style="zoom:33%;" />

### 进程管理

#### 进程的查看

##### top（实时的查看进程的状态，以及系统 CPU、内存信息）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-124356.png" alt="截屏2021-10-10 下午8.43.39" style="zoom:33%;" />

- top第一行

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-122938.png" alt="截屏2021-10-10 下午8.28.21" style="zoom:33%;" />

- 第二行（进程情况）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-123508.png" alt="截屏2021-10-10 下午8.34.47" style="zoom:33%;" />

- 第三行（cpu使用情况统计）

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-123708.png" alt="截屏2021-10-10 下午8.36.49" style="zoom:33%;" />

- 第四行（内存使用情况）

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-123827.png" alt="截屏2021-10-10 下午8.38.05" style="zoom:33%;" />

- 第五行（交换区使用情况）

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-124011.png" alt="截屏2021-10-10 下午8.40.03" style="zoom:33%;" />

- > 系统中可用的物理内存最大值并不是 free 这个单一的值，而是 free   buffers   swap 中的 cached 的和。

- 进程详情

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-124144.png" alt="截屏2021-10-10 下午8.41.12" style="zoom:33%;" />

  > - NICE 值叫做静态优先级，是用户空间的一个优先级值，其取值范围是 -20 至 19。这个值越小，表示进程”优先级”越高，而值越大“优先级”越低。 nice 值中的 -20 到 19，中 -20 优先级最高， 0 是默认的值，而 19 优先级最低。
  >
  > - PR 值表示 Priority 值叫动态优先级，是进程在内核中实际的优先级值，进程优先级的取值范围是通过一个宏定义的，这个宏的名称是 MAX_PRIO，它的值为 140。 Linux 实际上实现了 140 个优先级范围，取值范围是从 0-139，这个值越小，优先级越高。而这其中的 0-99 是实时进程的值，而 100-139 是给用户的。
  > - VIRT 任务所使用的虚拟内存的总数，其中包含所有的代码，数据，共享库和被换出 swap 空间的页面等所占据空间的总数。

##### 查看cpu数目与核心数

```
#查看物理 CPU 的个数
cat /proc/cpuinfo | grep "physical id" | sort | uniq |wc -l

#每个 cpu 的核心数
cat /proc/cpuinfo | grep "physical id" | grep "0" | wc -l
```

##### ps （静态查看当前的进程信息）

```
ps -l
// 本次登陆bash的进程

ps aux
// 罗列出所有的进程信息

ps aux | grep zsh
// 利用grep进行进程查找

ps axjf
// 进程树状关系

ps -afxo user,ppid,pid,pgid,command
//自定义信息显示
```

- 参数解释 ![截屏2021-10-10 下午9.03.55](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-130418.png)

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-130524.png" alt="截屏2021-10-10 下午9.04.51" style="zoom:50%;" />

- STAT状态参数解释

  <img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-130607.png" alt="截屏2021-10-10 下午9.05.13" style="zoom:50%;" />

##### pstree （查看当前活跃进程的树形结构）

```
pstree
```

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-130709.png" alt="截屏2021-10-10 下午9.06.57" style="zoom:33%;" />

#### 进程操作

##### kill

```
# 首先我们使用图形界面打开了 gedit、gvim，用 ps 可以查看到
ps aux

# 使用 9 这个信号强制结束 gedit 进程
kill -9 1608

# 我们再查找这个进程的时候就找不到了
ps aux | grep gedit
```

##### nice（设置优先级）

> 而 nice 的值我们是可以通过 nice 命令来修改的，而需要注意的是 nice 值可以调整的范围是 -20 ~ 19，其中 root 有着至高无上的权力，既可以调整自己的进程也可以调整其他用户的程序，并且是所有的值都可以用，而普通用户只可以调制属于自己的进程，并且其使用的范围只能是 0 ~ 19，因为系统为了避免一般用户抢占系统资源而设置的一个限制

```
# 这个实验在环境中无法做，因为权限不够，可以自己在本地尝试

# 打开一个程序放在后台，或者用图形界面打开
nice -n -5 vim &

# 用 ps 查看其优先级
ps -afxo user,ppid,pid,stat,pri,ni,time,command | grep vim
```

- renice（重置已有进程优先级）

```
renice -5 pid
```



## Linux日志系统

### 常见日志

- Linux常见的日志一般存放在 /var/log 中

#### 日志分类

- 系统日志

> 系统日志主要是存放系统内置程序或系统内核之类的日志信息如 alternatives.log 、btmp 等等，应用日志主要是我们装的第三方应用所产生的日志如 tomcat7 、apache2 等等。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/2021-10-10-132147.png" alt="截屏2021-10-10 下午9.21.10" style="zoom:50%;" />

- 应用日志

### 配置的日志

> 自己产生目标需要的日志

rsyslog 的全称是 rocket-fast system for log，它提供了高性能，高安全功能和模块化设计。 rsyslog 能够接受各种各样的来源，将其输入，输出的结果到不同的目的地。 rsyslog 可以提供超过每秒一百万条消息给目标文件。

- 安装

```
sudo apt-get update
sudo apt-get install -y rsyslog
sudo service rsyslog start
ps aux | grep syslog
```

- 配置

```
vim /etc/rsyslog.conf

vim /etc/rsyslog.d/50-default.conf

// 第一个主要是配置的环境,也就是 syslog 加载什么模块,文件的所属者等;而第二个主要是配置的 Filter Conditions
```

### 转储的日志

- logrotate 程序是一个日志文件管理工具。用来把旧的日志文件删除，并创建新的日志文件。我们可以根据日志文件的大小，也可以根据其天数来切割日志、管理日志，这个过程又叫做“转储”。
- 配置文件

```
cat /etc/logrotate.conf
```

- 配置文件参数

```
# see "man logrotate" for details  //可以查看帮助文档
# rotate log files weekly
weekly                             //设置每周转储一次(daily、weekly、monthly当然可以使用这些参数每天、星期，月 )
# keep 4 weeks worth of backlogs
rotate 4                           //最多转储4次
# create new (empty) log files after rotating old ones
create                             //当转储后文件不存在时创建它
# uncomment this if you want your log files compressed
compress                          //通过gzip压缩方式转储（nocompress可以不压缩）
# RPM packages drop log rotation information into this directory
include /etc/logrotate.d           //其他日志文件的转储方式配置文件，包含在该目录下
# no packages own wtmp -- we'll rotate them here
/var/log/wtmp {                    //设置/var/log/wtmp日志文件的转储参数
    monthly                        //每月转储
    create 0664 root utmp          //转储后文件不存在时创建它，文件所有者为root，所属组为utmp，对应的权限为0664
    rotate 1                       //最多转储一次
}
```





