#关闭防火墙，关闭selinux


#介绍
从php5.3开始，php-fpm就已经集成到php中了，所以即使你用的nginx服务器，也不需要自己再去安装fastcgi或者php-fpm了

#说明
CGI:一种协议

FastCGI : 基于CGI协议的实现

php-fpm : FastCGI进程管理器


#依赖包
yum install gd-devel libjpeg-devel libpng-devel libxml2-devel bzip2-devel libcurl-devel openldap-devel curl-devel

自己编译时：编译libmcrypt (然后指定到--with-mcrypt=/usr/local/libmcrypt)

#解压
tar -zxvf php-5.5.18.tar.gz
cd php-5.5.18


#设置配置文件
./configure --prefix=/usr/local/php-5.5.18 \
--enable-fpm \
--with-mcrypt=/usr/local/libmcrypt \
--with-zlib \
--enable-mbstring \
--with-openssl \
--with-mysql \
--with-mysqli \
--with-mysql-sock \
--with-gd \
--enable-gd-native-ttf \
--enable-pdo \
--with-pdo-mysql \
--with-gettext \
--with-curl \
--with-pdo-mysql \
--enable-sockets \
--enable-bcmath \
--enable-xml \
--enable-zip \
--enable-freetype

#编译
make;
make test;
make install;

#验证安装
cd /usr/local/php-5.5.18/bin
./php -v  查看php版本


#启动php通讯服务

#复制配置php-fpm文件
cp -arip /usr/local/php-5.5.18/etc/php-fpm.conf.default /usr/local/php-5.5.18/etc/php-fpm.conf

#复制php.ini文件（编译包种的文件）
cp -airp /usr/local/src/php-5.5.18/php.ini-production /usr/local/php-5.5.18/etc/php.ini
ln -s /usr/local/php-5.5.18/etc/php.ini /usr/local/php-5.5.18/lib/ #软链一份到lib目录中

#php.ini
注意php.ini 有时会加载不到，这时要查看phpinfo.php，看看php.ini加载目录在哪儿。
修改了php.ini，重新启动php-fpm就会生效


#修改fpm配置文件
vim /usr/local/php-5.5.18/etc/php-fpm.conf

#配置用户
user = evans
group = evans

#配置socket文件 （主要用于通讯）
pid = /usr/local/php-5.5.18/var/run/php-fpm.pid

#监听端口
listen = 127.0.0.1:9000

#启动服务
/usr/local/php-5.5.18/sbin/php-fpm
#删除进程
ps -aux | grep php-fpm 或者 pstree -Aup | grep php-fpm
kill -15 pid


#软链
ln -s /usr/local/php-5.5.18/sbin/php-fpm /usr/local/bin/
ln -s /usr/local/php-5.5.18/bin/* /usr/local/bin/

netstat -tunlp
