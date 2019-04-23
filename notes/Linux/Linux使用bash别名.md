# Linux使用bash别名

------------

对于Linux下使用**bash别名**，其主要的一个作用就是可以将平时比较复杂的命令使用简短的命令来进行替换。

主要使用**alias**命令

修改的文件**`.bash_profile`**以及**`.bashrc`**，对于这两个文件进行修改都可以起到使用bash别名的效果。

**`.bash_profile`**：对于Linux下的每一个用户都起着修改的作用。本质是在Linux启动时，**`.bash_profile`** 会自动加载 **`.bashrc`**文件。所以可以直接修改 **`bash_profile`** 文件

如下图：

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/Linux/%E5%91%BD%E4%BB%A4/alias/alias.png)

这就是**`bash_profile`** 文件里面调用了当前用户下的 **`.bashrc`**文件使用别名。

**`.bashrc`**：对于每一个用户都有着自己的一个**`.bashrc`** 文件，因此修改 **`.bashrc`**文件修改的仅仅只是针对的单个用户来进行说明的

**alias的示例：**

``` C++
//可以解压任何.tar的压缩文件
alias untar='tar -zxvf'
//简化clear操作
alias c='clear'
```

**以上添加别名**

接下来说一下如何来进行**别名的删除**

对于别名的删除，有着两种删除一种是暂时删除，一种是永久删除

**暂时删除**：对于暂时删除，只需要使用 **`unalias 命令名`** 即可。比如： **`unalias c`**

**永久删除**：对于永久删除，我们就需要修改 **`bash_profile`** 或者 **`bashrc`**文件才可以进行永久删除。

永久删除分为两步：

1. 首先在上述的两个文件中删除这个别名的语句
2. 其次使用alias，可以查看到我们现在有哪些别名，随后使用 **`unalias 文件名`** 将别名进行删除即可

