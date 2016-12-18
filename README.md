# TCP-BBR

BBR，google出品的加速优化，与锐速类似。限KVM、XEN使用。
均在SSH下执行。买了vps搞ss加速是不错的，可以试下，国人的宽带里有人测试认为联通效果最好的，电信一般，移动本身就还可以。不过各地的情况都一样的，可以自测为准。

一、centos6 32位和64位 下一键安装

wget https://zhujiwiki.com/usr/uploads/2016/12/BBR.sh && sh BBR.sh.
建议使用centos6 32位。

二、centos7 下安装

1、安装

wget http://mirrors.kernel.org/debian/pool/main/l/linux/linux-image-4.9.0-rc8-amd64-un.
signed_4.9~rc8-1~exp1_amd64.deb.
ar x linux-image-4.9.0-rc8-amd64-unsigned_4.9~rc8-1~exp1_amd64.deb.
tar -Jxf data.tar.xz.
install -m644 boot/vmlinuz-4.9.0-rc8-amd64 /boot/vmlinuz-4.9.0-rc8-amd64.
cp -Rav lib/modules/4.9.0-rc8-amd64 /lib/modules/.
depmod -a 4.9.0-rc8-amd64.
#centos >= 6.
dracut -f -v --hostonly -k '/lib/modules/4.9.0-rc8-amd64'  /boot/initramfs-4.9.0-rc8-a.
md64.img 4.9.0-rc8-amd64.
grub2-mkconfig -o /boot/grub2/grub.cfg.
2、修改启动顺序
vi /boot/grub2/grub.cfg.
把4.9.0的内核启动，放到第一位。

3、重启

4、开启BBR

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
sysctl net.ipv4.tcp_available_congestion_control
三、Debian8/Ubuntu14 下安装

1、安装
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9-rc8/linux-image-4.9.0-040900rc8-generic_4.9.0-040900rc8.201612051443_amd64.deb
dpkg -i linux-image-4.9.0*.deb

2、删除其余内核

dpkg -l|grep linux-image 
apt-get remove 内核
3、更新 grub 系统引导文件并重启

update-grub
reboot
4、开启BBR

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
5、保存，生效

sysctl -p
6、查看成功否
执行sysctl net.ipv4.tcp_available_congestion_control
如果结果中有bbr, 则证明你的内核已开启bbr
执行lsmod | grep bbr, 看到有 tcp_bbr 模块即说明bbr已启动＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝
