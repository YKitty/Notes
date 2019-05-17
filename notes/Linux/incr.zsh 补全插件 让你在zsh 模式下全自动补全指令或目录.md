# incr.zsh 补全插件 让你在zsh 模式下全自动补全指令或目录

![index.png](http://yijiebuyi.com/file/22d09b6e8aa35ed4180887e804526cb2)

前面有几篇[介绍了如何开启zsh终极装逼模式](http://yijiebuyi.com/blog/b9b5e1ebb719f22475c38c4819ab8151.html),如果你已经开启,请好好的装下去.

今天这个插件会助你一臂之力,加油,骚年!

今天说的这个东东看上去确实有尿性,不管我如何修饰都抵不过它的一张效果图.

![index.png](http://yijiebuyi.com/file/bafd0e6000495e627cb04a31947c55e5)

想要你的shell有这样的效果,首先满足下面的条件:

1  你用 oh-my-zsh 来协助你完成 zsh 的配置

2  你开启了 zsh

3  你下载了这个 插件

4  你把插件执行shell 写到了你的 .zshrc 配置文件中

上面说到的 1,2 在之前的博文里面早,最上面其实我贴出来如何开启 zsh 的链接.(如果你没有鼠标的话,肯定点不开)

今天就是分享下 3,4 提到的内容:

如何下载这个插件:

官网: <http://mimosa-pudica.net/zsh-incremental.html>

点开后你的第一反应应该是一个F开头的单词蹦出来,我当时也是.

我下载的本本是 [incr-0.2.zsh](http://mimosa-pudica.net/src/incr-0.2.zsh) 

如果某一天这个官网由于种种原因你访问不到了....所以我把 incr-0.2.zsh 的脚本贴出来一份就当备份

```
# Incremental completion for zsh
# by y.fujii <y-fujii at mimosa-pudica.net>, public domain


autoload -U compinit
zle -N self-insert self-insert-incr
zle -N vi-cmd-mode-incr
zle -N vi-backward-delete-char-incr
zle -N backward-delete-char-incr
zle -N expand-or-complete-prefix-incr
compinit

bindkey -M viins '^[' vi-cmd-mode-incr
bindkey -M viins '^h' vi-backward-delete-char-incr
bindkey -M viins '^?' vi-backward-delete-char-incr
bindkey -M viins '^i' expand-or-complete-prefix-incr
bindkey -M emacs '^h' backward-delete-char-incr
bindkey -M emacs '^?' backward-delete-char-incr
bindkey -M emacs '^i' expand-or-complete-prefix-incr

unsetopt automenu
compdef -d scp
compdef -d tar
compdef -d make
compdef -d java
compdef -d svn
compdef -d cvs

# TODO:
#     cp dir/

now_predict=0

function limit-completion
{
	if ((compstate[nmatches] <= 1)); then
		zle -M ""
	elif ((compstate[list_lines] > 6)); then
		compstate[list]=""
		zle -M "too many matches."
	fi
}

function correct-prediction
{
	if ((now_predict == 1)); then
		if [[ "$BUFFER" != "$buffer_prd" ]] || ((CURSOR != cursor_org)); then
			now_predict=0
		fi
	fi
}

function remove-prediction
{
	if ((now_predict == 1)); then
		BUFFER="$buffer_org"
		now_predict=0
	fi
}

function show-prediction
{
	# assert(now_predict == 0)
	if
		((PENDING == 0)) &&
		((CURSOR > 1)) &&
		[[ "$PREBUFFER" == "" ]] &&
		[[ "$BUFFER[CURSOR]" != " " ]]
	then
		cursor_org="$CURSOR"
		buffer_org="$BUFFER"
		comppostfuncs=(limit-completion)
		zle complete-word
		cursor_prd="$CURSOR"
		buffer_prd="$BUFFER"
		if [[ "$buffer_org[1,cursor_org]" == "$buffer_prd[1,cursor_org]" ]]; then
			CURSOR="$cursor_org"
			if [[ "$buffer_org" != "$buffer_prd" ]] || ((cursor_org != cursor_prd)); then
				now_predict=1
			fi
		else
			BUFFER="$buffer_org"
			CURSOR="$cursor_org"
		fi
		echo -n "\e[32m"
	else
		zle -M ""
	fi
}

function preexec
{
	echo -n "\e[39m"
}

function vi-cmd-mode-incr
{
	correct-prediction
	remove-prediction
	zle vi-cmd-mode
}

function self-insert-incr
{
	correct-prediction
	remove-prediction
	if zle .self-insert; then
		show-prediction
	fi
}

function vi-backward-delete-char-incr
{
	correct-prediction
	remove-prediction
	if zle vi-backward-delete-char; then
		show-prediction
	fi
}

function backward-delete-char-incr
{
	correct-prediction
	remove-prediction
	if zle backward-delete-char; then
		show-prediction
	fi
}

function expand-or-complete-prefix-incr
{
	correct-prediction
	if ((now_predict == 1)); then
		CURSOR="$cursor_prd"
		now_predict=0
		comppostfuncs=(limit-completion)
		zle list-choices
	else
		remove-prediction
		zle expand-or-complete-prefix
	fi
}
```

脚本内容结束.

如何使用?

首先去你官网或者从上面把这个 shell 文件内容copy 下来,然后放到一个指定的目录下.

该放到什么目录下,这个看你的个人爱好,我推荐你放在 oh-my-zsh 目录的插件目录下

```
➜  /Users/zhangzhi  >.oh-my-zsh/plugins/
```

然后新建一个 incr 目录,最后在 incr 目录下创建一个shell 文档,把你copy 的内容放进去.

上面的操作有几个注意点: 创建文件夹用sudo 权限.

创建完的 shell 文档要给赋 777 权限.

```
➜  /Users/zhangzhi/.oh-my-zsh/plugins/incr git:(master) ✗ >ls
incr-0.2.zsh
```

上面就是我放置 incr 插件的目录.

然后是配置 .zshrc 文件:

```
➜  /Users/zhangzhi  >nano .zshrc
```

然后插入一句下面的 shell 脚本:

```
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
```

我把她放在了配置文件最下方.

请注意上面的路径地址,你要改成你的 incr*.zsh 所在的目录

出自:[incr.zsh 补全插件 让你在zsh 模式下全自动补全指令或目录](http://yijiebuyi.com/blog/36955b84c57e338dd8255070b80829bf.html)

 

然后保存 .zshrc 配置文件,这时如果你想让它立即生效

```
➜  /Users/zhangzhi  >source .zshrc
```

这样就可以了.其他 shell 窗口可以关闭重新打开及有了补全提示.

它不仅仅是对指令的补全,而且也会补全路径,文件名,最重要的是实时的,掌握好你的 tab 键,去飞吧!



参考资料：<http://yijiebuyi.com/blog/36955b84c57e338dd8255070b80829bf.html> 