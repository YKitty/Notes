# git fetch和git pull的区别

---------

1、git fetch 相当于是从远程获取最新到本地，不会自动merge，如下指令：

```
　git fetch orgin master //将远程仓库的master分支下载到本地当前branch中

　git log -p master  ..origin/master //比较本地的master分支和origin/master分支的差别

　git merge origin/master //进行合并
```

也可以用以下指令：

```
git fetch origin master:tmp //从远程仓库master分支获取最新，在本地建立tmp分支

git diff tmp //將當前分支和tmp進行對比

git merge tmp //合并tmp分支到当前分支
```

 

2. git pull：相当于是从远程获取最新版本并merge到本地

```
git pull origin master
```

git pull 相当于从远程获取最新版本并merge到本地

在实际使用中，git fetch更安全一些

参考资料：<https://www.cnblogs.com/qiu-Ann/p/7902855.html> 