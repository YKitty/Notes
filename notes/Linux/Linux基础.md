# Linux基础知识

----------

## 1. 帮助

### 1.1 --help

一般指令的基本用法与选项介绍

### 1.2 man

使用man手册进行查找。对于man手册的使用采用**`man man`**即可

[man手册介绍](https://github.com/YKitty/Notes/blob/master/notes/Linux/bash%E8%AF%A6%E8%A7%A3.md#3-man%E8%AF%A6%E8%A7%A3 )

### 1.3 doc

/usr/share/doc下存放软件的一整套说明文件

## 2. vim

Linux下没有Windows下的IDE（集成开发环境），Linux执行一个程序预处理，编译，汇编，链接这四部分是分开的，有不同的程序完成

Linux下编写代码就只有一个**文本编辑器vim**

对于vim学习与配置：

1. 终端使用**vimtutor命令**
2. [学习](https://vim.ink/)

## 3. 目录结构

- bin：常用的命令
- boot：启动Linux系统过程中需要用到的文件
- dev：Linux系统中使用到的外部设备文件（Linux下一切皆文件）
- etc：各种配置文件和子目录
- sbin：系统管理员的系统管理程序
- home：存放普通用户的主目录
- lib：存放系统动态链接库
- mnt：用户可将别的文件系统挂载，比如另外的硬盘
- proc：虚拟目录，系统内存的映射，获取Linux系统信息
- root：超级用户的主目录
- usr：用户的应用程序和文件，类似于Windows下的program files
  - bin：存放系统用户使用的应用程序
  - sbin：存放超级用户所使用的高级程序以及系统守护程序
  - src：内核源代码默认放置目录
- tmp：存放临时文件
- run：系统运行需要的文件
- var：经常被修改的文件，比如日志文件，电子邮件等

## 4. 网络

### 4.1 DNS配置

**`vim /etc/resolv.conf`**

### 4.2 网络配置

**`vim /etc/sysconfig/network-scripts/ifcfg-ens33 `**

模板配置：

``` C++
TYPE=Ethernet                                                 PROXY_METHOD=none    
BROWSER_ONLY=no    
BOOTPROTO=dhcp    
DEFROUTE=yes             
IPV4_FAILURE_FATAL=no    
IPV6INIT=yes         
IPV6_AUTOCONF=yes    
IPV6_DEFROUTE=yes        
IPV6_FAILURE_FATAL=no                
IPV6_ADDR_GEN_MODE=stable-privacy    
NAME=ens33    
UUID=b9d080ce-f726-41fa-9a27-ef64a6aff8d7    
DEVICE=ens33    
ONBOOT=yes    
```



## 5. sudo命令

1. 切换到root用户下。su
2. 为/etc/sudoers添加执行权限。chmod +x /etc/sudoers
3. 在/etc/soduers中的root （ALL）=（ALL）ALL下面添加。使用gedit编辑器
   1. 用户名 （ALL）= （ALL）ALL即可退出保存

