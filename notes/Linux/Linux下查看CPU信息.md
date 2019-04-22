# Linux下查看CPU信息

----------------

在Linux操作系统中，CPU的信息在**启动**时就被装载到**虚拟目录/proc**下的**cpuinfo**文件中，我们可以通过**cat /proc/cpuinfo**来进行查看

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/proc/cpuinfo.png)

对于其中比较重要的指标：

**processor：逻辑处理器id，对于每一个逻辑处理器都有着自己的一个id**

**physical id：物理处理器id**

**core id：内核id**

**cpu cores：内核个数**

**siblings：对于一个物理处理器有着几个逻辑处理器**

从上图看，我的physical id为0表示，只有一个物理处理器，cpu cores是1，表明是单核的。说明我的CPU是单核的并且每一个核心只有一个逻辑处理器

下来举例一个工作站的服务器：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/proc/cpuinfo2.jpg)

physical id：0和1两个，表示有两个物理处理器

cpu cores：4，表示每一个物理处理器是4核的

core id：8个，对于一个物理处理器有着8个核序号，表示每个cpu core有划分为2个逻辑处理器(超线程技术)

siblings：8个，表示一个物理处理器有着8个逻辑处理器

processor：16个，表示对于这个CPU一共有着16个逻辑处理器