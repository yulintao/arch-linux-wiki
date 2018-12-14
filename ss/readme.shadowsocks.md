# archlinux安装shadowsocks等代理自启动方法：

```
git clone https://github.com/shadowsocks/shadowsocks.git -b master
下面为一个ss-client的配置文件,配置文件放到/etc/shadowsocks/config.json下面
{
    "server":"serverip",
    "server_port":1234,
    "local_port":1234,
    "local_address":"0.0.0.0",
    "password":"password",
    "timeout":600,
    "method":"encrypt-method"
}

将shadowsocks.service复制到/lib/systemd/syste/下面，
然后执行命令：
sudo systemctl enable shadowsocks 
sudo systemctl start shadowsocks

安装polipo代理本地不支持socks5代理的程序

默认配置文件目录为:/etc/polipo/修改其中参数为：

socksParentProxy = "localhost:8888"
socksProxyType = socks5
proxyPort = 8123
# proxyAddress = "::0"        # both IPv4 and IPv6
proxyAddress = "0.0.0.0"    # IPv4 only

sudo systemctl enable polipo
sudo systemctl start polipo 



```
