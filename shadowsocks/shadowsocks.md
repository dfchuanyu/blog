默认配置：
服务器端口：自己设定（如不设定，默认为 8989）
密码：自己设定（如不设定，默认为teddysun.com）
加密方式：自己设定（如不设定，默认为 aes-256-gcm）

客户端下载：
https://github.com/shadowsocks/shadowsocks-windows/releases

使用方法：
使用root用户登录，运行以下命令：
* CentOS

wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log

* Debian && Ubuntu

wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh
chmod +x shadowsocks-libev-debian.sh
./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log

安装完成后，脚本提示如下：
* Congratulations, shadowsocks-libev install completed!
* Your Server IP:your_server_ip
* Your Server Port:your_server_port
* Your Password:your_password
* Your Local IP:127.0.0.1
* Your Local Port:1080
* Your Encryption Method:aes-256-cfb

Welcome to visit:http://teddysun.com/357.html
Enjoy it!

卸载方法：
使用 root 用户登录，运行以下命令： ./shadowsocks-libev.sh uninstall

使用命令：
* 启动：/etc/init.d/shadowsocks start
* 停止：/etc/init.d/shadowsocks stop
* 重启：/etc/init.d/shadowsocks restart
* 查看状态：/etc/init.d/shadowsocks status

参考链接：
https://github.com/madeye/shadowsocks-libev
https://shadowsocks.be/4.html
