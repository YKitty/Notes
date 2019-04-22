# touch命令

------------

对于touch命令最为熟悉的就是两个用法：

1. **创建新文件**
2. **修改时间戳**

对于创建新文件，touch后面跟上文件名，如果文件不存在的话，创建出一个文件，文件存在的话，就会改变文件的时间戳。

如下图：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch.png)

如图可以看出，首先使用touch命令创建出一个新的文件test.cc，随后使用touch violence.cc改变了文件的时间戳。

接下来讲解一下其他用法：

## 1. 防止创建文件

对于一个文件我们只想改变其时间戳，如果这个文件不存在的话也不创建这个文件。

这里只需要加上**-c选项**即可

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-c.png)

这里可以看到，对于刚开始没有test.cc文件，我们使用**touch -c test.cc**之后，我们想改变这个文件的时间戳，但是没有这个文件也没有创建这个文件。

## 2. 仅改变文件访问时间

我们知道当我们使用touch之后，修改的**访问时间和修改时间**都会被改变成当前系统的时间。**对于访问时间以及修改时间的改变都会改变文件的属性时间，这是因为这两个都存在与属性当中。**

如图：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/stat%20touch.png)

可以看到使用touch命令之后，文件的访问时间和修改时间都发生了改变

如果我们只想**修改访问时间**加上**-a**即可。

如图：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-a.png)

**只改变了文件的访问时间**

## 3. 仅修改文件修改时间

如果我们要修改**文件的修改时间**只需要添加上**-m**即可

如图：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-m.png)

**只改变文件的修改时间**

## 4. 更改为自定义时间戳

不管是如何使用touch -a -m等都会改变为当前系统时间戳，那么我们如何改变为自己的时间戳呢？

我们有两种方法改变为自定义时间戳

### 4.1 加上-t选项

-t后面的时间戳的格式为：

``` C++
[[CC]YY]MMDDhhmm [.SS]
```

具体解释：

CC：年份的前两位

YY：年份的后两位

MM：月份 [01-12]

DD：日期 [01-31]

hh：时 [00-23]

mm：分 [00-59]

SS：秒 [00-60]

举例：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-t.png)

### 4.1 加上-d选项

在这里时间格式为：**`日-月-年`**。但是这里的时间也是相当的灵活，比如支持**yesterday、1 year ago等等模糊时间**

**除了可以更改时间，也可以更改时区。更改时区时，只需要-d后面跟上对应的时区即可**

举例：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-d.png)

**使用touch -d进行修改时，精准到天**

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/touch/touch%20-d.png)

从上图可以看出，对于**touch -d可以使用模糊时间来进行修改**