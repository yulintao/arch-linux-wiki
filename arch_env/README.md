

安装deb包

参考网址为：[makepkg安装deb](https://wiki.archlinux.org/index.php/makepkg)

安装形如skype类型的deb包到archlinux里面时需要用下面几个命令
```
git clone https://aur.archlinux.org/skypeforlinux-stable-bin.git
makepkg -S xxx.tar.gz
makepkg --syncdeps
makepkg --install 或者 pacman -U pkgname-pkgver.pkg.tar.xz
```
安装完成teamviewer以后需要运行下列命令，解决服务器为准备好问题
```
sudo teamviewer --daemon enable
```
archlinux 需要安装lightdm lightdm-gtk-greeter作为启动器, 默认的xorg-xinit的图形界面启动方式会导致启动不了teamviewer
```
pacman -S lightdm lightdm-gtk-greeter

有时候出现lightdm启动不了的情况，可能系统启动太快了，LightDM 服务在图形驱动加载前就启动了，需要在配置文件中添加如下内容：

/etc/lightdm/lightdm.conf

[LightDM]
logind-check-graphical=true

/etc/lightdm/Xsession

#xrandr --output Virtual-1 --mode 1360x768
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
#exec fcitx &
exec $@
```

安装 **skypeforlinux-bin,vmware-workstation**

vmware-workstation 需要在```/etc/init.d/```目录建立启动脚本，所以需要  
创建这个init.d目录，同时在重启以后需要启动vmware需要手动运行  
```/etc/init.d/vmware start``` 脚本

**reload Urxvt 配置文件**
```
xrdb -merge ~/.Xresources
```
**截图工具**
`flameshoot`

** 忘记密钥环 **

删掉```/home/`whoami`/.local/share/keyring```重新设置密钥

**安装软件时出现密钥不对的问题,(无法远程查找到密钥)**
原因： 由于升级到了 ```gnupg-2.1，pacman``` 上游更新了密钥环的格式，这使得本地的主密钥无法签署其它密钥  
解决办法： 首先安装```haveged``` 用来生成系统熵值的守护进程，包括生成新的密钥环

```
pacman -Syu haveged
systemctl start haveged
systemctl enable haveged

rm -fr /etc/pacman.d/gnupg

pacman-key --init
pacman-key --populate archlinux
pacman-key --populate archlinuxcn
```
**终端显示英文:**

```
 if [ "$TERM"="linux" ] ;then
    export LANG="en_US.UTF-8" 
 fi                          
```

**设置分辨率:**
```
xrandr --output HDMI-1 --mode 1920x1080
```
**设置多个显示器**
```
arandr 设置多显示器
(https://wiki.archlinux.org/index.php/xrandr#Manage_2-monitors)
(https://wiki.archlinux.org/index.php/xrandr)

```

**安全删除移动硬盘**
```
pacman -S udisks2
udisksctl power-off
```

**禁用触摸板**
需要用到这两个包：```libinput xf86-input-libinput```

然后将```/usr/share/X11/xorg.conf.d/70-synaptics.conf``` 拷贝到```/etc/X11/xorg.conf.d```下面，  
```sudo ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/40-libinput.conf```  
然后在```.config/i3/config```里面设置一条```exec synclient TouchpadOff=1```  

**输入法不能用的问题：**
需要将下面几个写入到全局配置文件```/etc/profile```中
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
在进入tty以后显示方块，可以通过LANG=en_US.UTF-8命令来解决这个问题：

**字体 思源黑体:**
```
pacman -S adobe-source-han-sans-cn-fonts
```
**字体 思源宋体:**
```
pacman -S adobe-source-han-serif-cn-fonts
```

安装oh-my-zsh以后前面提示符显示方块问题解决：  
安装powerline字体：https://github.com/powerline/fonts  
下载字体后手动移动到```/usr/share/fonts/xx``` 对应的目录中，如果下载的是ttf的字体就移动到```/usr/share/fonts/ttf```目录  
,然后执行```fc-cache -f -v```重新部署缓存字体，然后查看对应的字体名称```fc-list |grep Powerline```会列出如下字体：  
```
/usr/share/fonts/TTF/Meslo LG M Regular for Powerline.ttf: Meslo LG M for Powerline:style=Regular
/usr/share/fonts/OTF/Source Code Pro for Powerline.otf: Source Code Pro for Powerline:style=Regular
```
**终端配置字体显示，把需要使用的字体名字写到~/.Xresources里面，例如：**
```
URxvt.font:xft:Source Code Pro:antialias=True:pixelsize=15,xft:SourceHanSerifCN-Medium:pixelsize=15,
xft:Noto Color Emoji:pixelsize=15,FontAwesome:style=Regular,xft:Meslo LG M for Powerline:pixelsize=15,
xft:Source Code Pro for Powerline:pixelsize=15

antialias=True 表示开启抗锯齿
pixelsize=15 表示字体大小
xft:Meslo LG M for Powerline 中xtf 表示此字体是 ttf/otf等等格式的字体，后面是从fc-list 中查到的字体名字
有多少字体都可以放到后面，写上对应字体的特性即可
```
**设置屏幕亮度：数字表示设置的亮度值**
```
echo 300 > /sys/class/backlight/intel_backlight/brightness
```

自定义dns，不被networkmanager修改/etc/resolv.conf，
方法一：
在 /etc/NetworkManager/system-connections 中对应的wifi配置文件中加入如下配置
[ipv4]
dns=127.0.0.1
ignore-auto-dns=true
method=auto
方法二：
/etc/resolv.conf.head 里面添加需要的dns

wifi 经常自动断开
将wifi电源管理关掉
sudo iwconfig wlp2s0 power off

install i3-wm
中文输入法 的问题，需要安装如下程序:
```
pacman -S fcitx fcitx-im 
```
然后在.xinitrc或者/etc/profile 里面添加如下几个变量声明，全局中文显示
```
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8
```
如果系统中有安装其他的无线管理类的工具，也可以用其他的管理工具代替，管理工具之间都是互斥的，同时只能用一个，例如：  
Connman,Netctl,NetworkManager,Wicd,Wifi Radar这些，参考[Wifi这一章](https://wiki.archlinux.org/index.php/Wireless_network_configuration_(简体中文)#Realtek)

**添加桌面背景图:**
```
feh --bg-scale /usr/share/background/Windows-10-blue-light_2560x1440.jpg
```

**将所有终端窗口边框隐藏,将下面加到i3/config里面**
```
new_window 1pixel
```

**nmcli 设置链接网络例如：***
```
nmcli device wifi connect xxx password ***
```


**禁止终端发出声音**
两种方法:
```
第一种：
# rmmod pcspkr

第二种：
# echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf

或者
alsamixer 选择 Beep 把他改成MM即可

```

pavucontrol 音量控制，图形界面


** Troubleshooting **
```
这里会列举一些常见问题的解决方案：
1、通过rxvt-unicode 远程服务器出现“rxvt-unicode-256color': unknown terminal type” 错误，
解决办法是在远程服务器的工作目录中创建'~/.terminfo/r/'目录，然后从本机的/usr/share/terminfo/r/目录
中拷贝rxvt-unicode rxvt-unicode-256color 到远程机器刚刚创建的目录中即可。

```
