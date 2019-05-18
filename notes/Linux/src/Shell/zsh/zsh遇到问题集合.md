# zsh遇到问题集合

----------

- Vim出现：_arguments:450: _vim_files: function definition file not found的问题解决

安装了zsh之后使用vim出现如下错误：

``` 
arguments:450: _vim_files: function definition file not found
_arguments:450: _vim_files: function definition file not found
_arguments:450: _vim_files: function definition file not found
```



解决方法：

rm ~/.zcompdump*

然后重新打开Terminal。



参考资料：https://www.cnblogs.com/EasonJim/p/7865546.html