# 安装oh-my-zsh

```

 sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
 chsh -s zsh `whoami`

 所有的zsh插件都在.zshrc文件的plugins=（）中添加


zsh 切换带有git库的目录时响应慢的问题，
可以使用git config --global oh-my-zsh.hide-status 1 关闭对status的检查
然后disable auto upgrade To disable automatic upgrades, set the following in your ~/.zshrc:DISABLE_AUTO_UPDATE=true


```

# 安装autojump

```
下载源码
git clone git://github.com/wting/autojump.git && cd autojump && ./install.py 

安装完成后会在用户目录生成 .autojump的目录，然后将下面一行添加到zshrc中

[[ -s /home/yult/.autojump/etc/profile.d/autojump.sh ]] && source /home/yult/.autojump/etc/profile.d/autojump.sh


```



