# Centos7.0 安装交叉编译环境

------------------

## 1. 开始

由于CentOS7安装的是64位(查询命令getconf LONG_BIT),因此需要安装32位libc和libncurses

yum -y install glibc.i686

yum -y install ncurses

## 2. 解压arm-linux-gcc

将arm-linux-gcc-4.3.2.tgz解压到/usr/local目录下，

其自动会生成arm/4.3.2的目录，就解压到这个目录了

## 3. 更改arm权限

```  
sudo chmod 777 arm/
```

## 4. 修改环境变量

修改/etc/profile文件（此文件属于系统级别的环境变量，设置在里面的东西对所有用户适用）：

输入命令：# vim /etc/profile

增加路径设置，在末尾添加如下,保存/etc/profile文件：
export PATH＝$PATH:/usr/local/arm/4.4.3/bin

## 5. 使环境变量生效

source /etc/profile

## 6. 检测是否成功

输入命令：# arm-linux-gcc -v

成功输出以下命令：

Using built-in specs.
Target: arm-none-linux-gnueabi
Configured with: /opt/FriendlyARM/mini2440/build-toolschain/working/src/gcc-4.4.3/configure --build=i386-build_redhat-linux-gnu --host=i386-build_redhat-linux-gnu --target=arm-none-linux-gnueabi --prefix=/opt/FriendlyARM/toolschain/4.4.3 --with-sysroot=/opt/FriendlyARM/toolschain/4.4.3/arm-none-linux-gnueabi//sys-root --enable-languages=c,c++ --disable-multilib --with-arch=armv4t --with-cpu=arm920t --with-tune=arm920t --with-float=soft --with-pkgversion=ctng-1.6.1 --disable-sjlj-exceptions --enable-__cxa_atexit --with-gmp=/opt/FriendlyARM/toolschain/4.4.3 --with-mpfr=/opt/FriendlyARM/toolschain/4.4.3 --with-ppl=/opt/FriendlyARM/toolschain/4.4.3 --with-cloog=/opt/FriendlyARM/toolschain/4.4.3 --with-mpc=/opt/FriendlyARM/toolschain/4.4.3 --with-local-prefix=/opt/FriendlyARM/toolschain/4.4.3/arm-none-linux-gnueabi//sys-root --disable-nls --enable-threads=posix --enable-symvers=gnu --enable-c99 --enable-long-long --enable-target-optspace
Thread model: posix
gcc version 4.4.3 (ctng-1.6.1)

