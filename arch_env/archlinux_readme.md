

安装deb包

参考网址为：[ makepkg安装deb ] (https://wiki.archlinux.org/index.php/makepkg)

安装形如skype类型的deb包到archlinux里面时需要用下面几个命令
```
git clone https://aur.archlinux.org/skypeforlinux-stable-bin.git
makepkg -S xxx.tar.gz
makepkg --syncdeps
makepkg --install 或者 pacman -U pkgname-pkgver.pkg.tar.xz
```

忘记密钥环

删掉/home/`whoami`/.local/share/keyring重新设置密钥


安装完成teamviewer以后需要运行下列命令，解决服务器为准备好问题
```
sudo teamviewer --daemon enable
```

安装 **skypeforlinux-bin,vmware-workstation**

vmware-workstation 需要在```/etc/init.d/```目录建立启动脚本，所以需要
创建这个init.d目录，同时在重启以后需要启动vmware需要手动运行```/etc/init.d/vmware start```
脚本

安装软件时出现密钥不对的问题,(无法远程查找到密钥)

```
原因：
由于升级到了 gnupg-2.1，pacman 上游更新了密钥环的格式，这使得本地的主密钥无法签署其它密钥
解决办法：
首先安装haveged 用来生成系统熵值的守护进程，包括生成新的密钥环
pacman -Syu haveged
systemctl start haveged
systemctl enable haveged

rm -fr /etc/pacman.d/gnupg
pacman-key --init
pacman-key --populate archlinux

pacman-key --populate archlinuxcn

```
终端显示英文:

```
 if [ "$TERM"="linux" ] ;then
    export LANG="en_US.UTF-8" 
 fi                          
```

