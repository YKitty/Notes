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



## 6. inode

### 6.1 使用inode号删除一个文件

直接删除：**`find ./* -inum 192712179 -delete`**

询问之后删除：**`find ./* -inum 1049741 -exec rm -i {} \; `**



## 7. 文件处理命令

### 7.1 mkdir

- **创建目录（make directory）**
- mkdir -p [目录名]
  - -p 递归创建

### 7.2 cd

- **切换所在目录（change directory）**
- cd [目录]
  - ~  家目录
  - 家目录 （直接使用目录）
  - -（上次的目录）
  - . 当前目录
  - .. 上级目录

### 7.3 pwd

- **显示当前目录**

### 7.4 rmdir

- **删除目录 （remove empty directory）**
- rmdir [目录名]

### 7.5 rm

- **删除文件或者目录（remove）**
- rm [文件或者目录]
  - -r 删除目录（递归）
  - -f 强制删除
- rm -rf [文件或者目录] 递归强制删除所有目录

### 7.6 cp

- **赋值命令 （copy）**
- cp [源文件]\[目标文件] 
  - -r 复制目录（递归复制），默认是赋值文件
  - -p 连带文件属性复制
  - -d 若源文件是链接文件，则赋值链接属性
  - -a 相当于-rpd

### 7.7 mv

- **移动文件或者改名（move）**
- mv [源文件或者目录]\[目标文件或者新文件名]

### 7.8 ln

- **链接命令，生成链接文件（link）**

#### 7.8.1 硬链接特征

- ln [源文件]\[目标文件]

- 拥有相同的i节点和存储block块，可以看做是同一个文件
- 可以通过i节点访问
- 不能跨分区（不能适用于多个文件系统）
- 不能针对目录使用
- 一般不使用

#### 7.8.2 软链接特征

- ln -s [源文件]\[目标文件]
  - -s 创建软链接
- 类似于windows下的快捷方式
- 软链接拥有自己的i节点和block块，但是数据块中存放的是源文件的文件名和i节点号，并没有实际的文件数据
- lrwxrwxrwx 软链接的文件权限都是777
- 修改任意一个文件，另一个都会改变
- 删除源文件，软链接不能使用
- 软链接源文件必须写绝对路径



## 8. 文件搜索命令

### 8.1 whereis

- 搜索命令所在路径以及帮助文档所在位置
- whereis 命令名 （whereis ls）
  - -b 只查找可执行文件
  - -m 只查找帮助文档

### 8.2 which

- 可以看到别名**which ls**
- 能看到的都是外部安装的命令，无法查看Shell自带的命令，如which cd

### 8.3 环境变量

- 定义的是系统搜索命令的路径
- echo $PATH

### 8.4 find

- 文件搜索命令
- find [搜索范围]\[搜索条件]

#### 8.4.1 按名称搜索(-name)

- 避免大范围的搜索，会非常消耗系统资源
- **find / -name aaa.log**

#### 8.4.2 通配符

- find是在系统当中搜索符合条件的文件名，如果需要匹配，使用通配符皮匹配，通配符是完全匹配
- 通配符
  - *****匹配任意内容
  - **？**匹配任意一个字符
  - **[]**匹配任意一个中括号内的字符

#### 8.4.3 不区分大小写(-i)

- 不区分大小写
- **find / -iname A.log**

#### 8.4.4 按所有者搜索(-user)

- **find / -iuser root**

#### 8.4.5 按时间搜索

- find / -mtime +5

| 参数  |       含义       |
| :---: | :--------------: |
| atime |   文件访问时间   |
| mtime | 文件内容修改时间 |
| ctime | 文件属性修改时间 |

| 参数 |       含义        |
| :--: | :---------------: |
|  -5  |  5天之内修改文件  |
|  5   | 5天前当天修改文件 |
|  +5  | 5天前之前修改文件 |

#### 8.4.6 按文件大小搜索(-size)

- **k小写，M大写**
- **find / -size 100k**

| 参数 |  含义  |
| :--: | :----: |
| -8k  | 小于8k |
|  8k  | 等于8k |
| +8k  | 大于8k |
| +8M  | 大于8M |

#### 8.4.7 按i节点搜索(-inum) 

- **find / -inum 123456**

#### 8.4.8 综合应用

- **find /tmp -size +10k -a -size -20k**
- 查找/tmp目录下，大于10KB并且小于20KB的文件
- -a and 逻辑与，两个条件都满足
- -o 逻辑或，两个条件只需要满足一个
- **find /tmp -size +10k -a -size -20k -exec ls -1h {} \;**
- **exec对上个命令的结果进行操作**

### 8.5 grep

- 在文件中匹配符合条件的字符串
- **grep “10” access.log**
  - -i 忽略大小写
  - -v 排除指定字符串
- find命令，在系统中搜索符合条件的文件名，如果需要匹配，使用通配符匹配，通配符是完全匹配
- grep命令在文件当中搜索符合条件的字符串，如果需要匹配使用正则表达式进行匹配，正则表达时包含匹配



## 8. 帮助命令

### 8.1 基本用法

- man命令 获取指定命令的帮助
- tldr命令 获取对于一个命令简化的解释

### 8.2 关键字搜索

- **man -k passwd**

### 8.3 shell内部帮助

- whereis找到就是外部，找不到就是内部
- **help cd**

## 9. 压缩和解压命令

**.zip .gz .bz2 .tar.gz .tar.bz2**

### 9.1 zip格式

- 压缩文件：zip 压缩文件名 源文件

- 压缩目录：zip -r 压缩文件名 源目录

- 解压：unzip 压缩文件名

- ``` C++
  mkdir test
  touch 1.c
  touch 2.c
  zip -r test.zip test
  unzip test.zip
  ```

### 9.2 gzip格式

|           命令           |            示例            |                  含义                   |
| :----------------------: | :------------------------: | :-------------------------------------: |
|       gzip 源文件        |         gzip a.txt         |  压缩为.gz格式的压缩文件，源文件会消失  |
| gzip -c 源文件> 压缩文件 | gzip -c yum.txt>yum.txt.gz | 压缩为.gz格式的压缩文件，源文件不会消失 |
|       gzip -r 目录       |        gzip -r test        |  压缩目录下的所有文件，但是不压缩目录   |
|    gzip -d 压缩文件名    |     gzip -d yum.txt.gz     |        解压缩文件，不保留压缩包         |
|     gunzip 压缩文件      |     gunzip yum.txt.gz      |        解压缩文件，不保留压缩包         |

### 9.3 bz2格式

|        命令         |        示例        |                含义                |
| :-----------------: | :----------------: | :--------------------------------: |
|    bzip2 源文件     |    bzip2 1.txt     | 压缩为.bz2格式的文件，不保留源文件 |
|   bzip2 -k 源文件   |   zip2 -k 1.txt    |  压缩为.bz2格式的文件，保留源文件  |
| bzip2 -d 压缩文件名 | bzip2 -d 1.txt.bz2 |             解压压缩包             |
| bunzip2 压缩文件名  | bunzip2 1.txt.bz2  |             解压压缩包             |

- **bzip不能压缩目录**

### 9.4 tar

#### 9.4.1 打包命令

- tar -cvf 打包文件名 源文件

  - -c 打包
  - -v 显示过程
  - -f 指定打包后的文件名

- ``` C++
  //进行打包
  tar -cvf book.tar book
  //采用gzip方式压缩
  gzip book.tar
  //采用bz2方式压缩
  bzip2 book.tar
  ```

- **-x解打包**

- ``` c++
  tar -xvf book.tar
  ```

#### 9.4.2 压缩命令

- **压缩**

``` C++
//将目录里所有jpg文件打包成jpg.tar
tar -cvf jpg.tar *.jpg
//将目录下所有jpg文件打包成jpg.tar之后，并将其使用gzip格式进行压缩，生成一个gzip压缩包，命名为jpg.tar.gz
tar -czf jpg.tar.gz *.jpg
//将目录下所有jpg文件打包成jpg.tar之后，并将其使用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
tar -cjf jpg.tar.bz2 *.jpg
//将目录下所有的jpg文件打包成jpg.tar之后，并将其使用compress压缩，生成一个uncompress压缩过的包，命名为jpg.tar.Z
tar -cZf jpg.tar.Z *.jpg
//rar格式的压缩，需要先下载rar for Linux
rar	a jpg.rar *.jpg
//zip格式的压缩，需要先下载zip for Linux
zip jpg.zip *.jpg
```

- **解压**

``` C++
//解压tar包
tar -xvf file.tar
//解压tar.gz
tar -xzvf file.tar.gz
//解压tar.bz2
tar -xjvf file.tar.bz2
//解压tar.Z
tar -xZvf file.tar.Z
//解压rar
unrar e file.tar
//解压zip
unzip file.zip
```



