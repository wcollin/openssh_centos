由于安全漏扫需要，yum update后也无法解决，所以想自己做个openssh的rpm包来解决。

1. 官网下载最新版的openssh包，地址：https://www.openssh.com/portable.html 如openssh-8.1p1.tar.gz

2. 下载centos当前大版本的最新版的src.rpm包（需要里面的补丁，也可以参考里面的openssh.spec），centos7建议最少增加systemd和tcp_wrapper的支持。
地址：http://vault.centos.org/7.7.1908/os/Source/SPackages/openssh-7.4p1-21.el7.src.rpm   如openssh-7.4p1-21.el7.src.rpm

使用命令进行解压：rpm2cpio openssh-7.4p1-21.el7.src.rpm |cpio -iv 
获取如下信息：
a. openssh.spec
b. openssh-6.6p1-systemd.patch
c. openssh-7.4p1-debian-restore-tcp-wrappers.patch

3. 安装编译依赖包
yum install rpm-build gcc make wget openssl-devel krb5-devel pam-devel libX11-devel xmkmf libXt-devel autoconf automake libtool -y

4. 创建编译目录：
mkdir -p /root/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}

5. 将openssh.spec和补丁放到/root/rpmbuild/SOURCES目录下，然后在/root/rpmbuild/SOURCES目录下执行rpmbuild -bb openssh.spec

6. 正常情况下生成的包在/root/rpmbuild/RPMS/x86_64目录下

7. 升级测试是否正常

