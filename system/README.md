这个目录主要是安装系统时修改的系统配置。

基本上都是直接拷贝过去即可，对于图形界面，lightdm而言，还需要通过systemctl设置自启动。命令为:
sudo systemctl enable lightdm && sudo systemctl start lightdm

之后所有需要自动启动的程序都可以直接通过systemctl 进行启动，或者安装一个supervisor，将配置放到/etc/supervisor.d/里面
