

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
archlinux 需要安装lightdm lightdm-gtk-greeter作为启动器,
用以解决启动teamviewer时候启动不了的问题，默认的xorg的方式会导致启动不料teamviewer

安装 **skypeforlinux-bin,vmware-workstation**

vmware-workstation 需要在```/etc/init.d/```目录建立启动脚本，所以需要
创建这个init.d目录，同时在重启以后需要启动vmware需要手动运行```/etc/init.d/vmware start```
脚本

安装软件时出现密钥不对的问题,(无法远程查找到密钥)
原因：
由于升级到了 gnupg-2.1，pacman 上游更新了密钥环的格式，这使得本地的主密钥无法签署其它密钥
解决办法：
首先安装haveged 用来生成系统熵值的守护进程，包括生成新的密钥环

```
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

设置分辨率:
```
xrandr --output HDMI-1 --mode 1920x1080
```
设置多个显示器
```
(https://wiki.archlinux.org/index.php/xrandr#Manage_2-monitors)
(https://wiki.archlinux.org/index.php/xrandr)
```

disable touchpad
需要用到这两个包：libinput xf86-input-libinput
然后将/usr/share/X11/xorg.conf.d/70-synaptics.conf 拷贝到/etc/X11/xorg.conf.d下面，
然后在.config/i3/config里面设置一条exec synclient TouchpadOff=1

输入法不能用的问题：
需要将下面几个写入到全局配置文件中
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
在进入tty以后显示方块，可以通过LANG=en_US.UTF-8命令来解决这个问题：

安装oh-my-zsh以后前面提示符显示方块问题解决：
安装powerline字体：https://github.com/powerline/fonts
下载字体后手动移动到/usr/share/fonts/xx 对应的目录中，如果下载的是ttf的字体就移动到/usr/share/fonts/ttf目录
,然后执行fc-cache -f -v重新部署缓存字体，然后查看对应的字体名称fc-list |grep Powerline会列出如下字体：
```
/usr/share/fonts/TTF/Meslo LG M Regular for Powerline.ttf: Meslo LG M for Powerline:style=Regular
/usr/share/fonts/OTF/Source Code Pro for Powerline.otf: Source Code Pro for Powerline:style=Regular
```
把需要使用的字体名字写道~/.Xresources里面，例如：
```
URxvt.font:xft:Source Code Pro:antialias=True:pixelsize=15,xft:SourceHanSerifCN-Medium:pixelsize=15,xft:Noto Color Emoji:pixelsize=15,FontAwesome:style=Regular,xft:Meslo LG M for Powerline:pixelsize=15,xft:Source Code Pro for Powerline:pixelsize=15

```
设置屏幕亮度：数字表示设置的亮度值
```
echo 500 > /sys/class/backlight/intel_backlight/brightness
```

字体 思源黑体:
```
pacman -S adobe-source-han-sans-cn-fonts
```
字体 思源宋体:
```
pacman -S adobe-source-han-serif-cn-fonts
```

install i3-wm
中文输入法 的问题，需要安装如下程序:
```
pacman -S fcitx fcitx-im 
```
然后在.xinitrc或者/etc/profile 里面添加如下几个变量声明
```
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8
```

添加桌面背景图:
```
feh --bg-scale /usr/share/background/Windows-10-blue-light_2560x1440.jpg
```

将所有终端窗口边框隐藏,将下面加到i3/config里面
```
new_window 1pixel
```

