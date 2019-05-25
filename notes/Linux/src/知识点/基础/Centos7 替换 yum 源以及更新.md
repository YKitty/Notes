# Centos7 替换 yum 源以及更新

-------------------

[1] 首先备份

```
/etc/yum.repos.d/CentOS-Base.repo
```

 

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

 

[2] 进入yum源配置文件所在文件夹

```
[root@localhost yum.repos.d]# cd /etc/yum.repos.d/
```

 

[3] 下载163的yum源配置文件，放入/etc/yum.repos.d/(操作前请做好相应备份)

对于那一个版本的Centos就下载对应的源：这里是默认下载 **6版本** 的

```
[root@localhost yum.repos.d]# wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
```

 如果下载 **7版本的** ： 

``` 
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
```



对于下面也可以下载其他的yum源来进行使用：这里是阿里云的yum源

#### （CentOS 5

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```

**CentOS 6**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
```

**CentOS 7**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo）
```

 

[4] 运行yum makecache生成缓存

```
[root@localhost yum.repos.d]# yum makecache
```

 

[5] 更新系统

```
[root@localhost yum.repos.d]# yum -y update
```

 

FAQ：

1. [yum提示Another app is currently holding the yum lock; waiting for it to exit](http://blog.csdn.net/testcs_dn/article/details/48711805)

可以通过强制关掉yum进程：

```
rm -f /var/run/yum.pid
```

 

参考资料：[博客：深蓝至尊](https://www.cnblogs.com/shenlanzhizun/p/7683166.html)





