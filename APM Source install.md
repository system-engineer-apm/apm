# APM Source 설치
### EPEL 저장소를 설치
```
sudo amazon-linux-extras install epel -y
```

### APM 컴파일을 위한 의존성 패키지 설치
```
sudo yum groupinstall "Development Tools" -y
yum -y install make gcc gcc-c++ ncurses-devel libevent openssl openssl-devel gnutls-devel libxml2 libxml2-devel bison gmp gmp-devel bzip2-devel curl-devel libjpeg-devel libXpm-devel
yum -y install libtool expat-devel pcre-devel apr-devel apr-util libzip pcre-devel expat-devel apr-util-devel libdb-devel libpng-devel freetype-devel cmake
```

### Cmake 컴파일 및 설치
```
wget https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1.tar.gz
tar -zxvf cmake-3.22.1.tar.gz
cd cmake-3.22.1
./bootstrap
make
make install
vi ~/.bash_profile
PATH=$PATH:/root/src/cmake-3.22.1/bin
```

### MariaDB 10.3.39 설치
```
mkdir -p /root/src
cd /root/src
wget https://archive.mariadb.org/mariadb-10.3.39/source/mariadb-10.3.39.tar.gz
tar xvfz mariadb-10.3.39.tar.gz
cd mariadb-10.3.39

cmake -DCMAKE_INSTALL_PREFIX=/opt/mysql -DMYSQL_DATADIR=/opt/mysql/var -DMYSQL_UNIX_ADDR=/tmp/mysql.sock -DENABLED_LOCAL_INFILE=ON -DWITH_SSL=system -DWITH_ZLIB=system -DWITH_JEMALLOC=no -DWITH_ARIA_STORAGE_ENGINE=ON -DUSE_ARIA_FOR_TMP_TABLES=ON -DWITH_PARTITION_STORAGE_ENGINE=ON -DWITH_PERFSCHEMA_STORAGE_ENGINE=ON -DWITH_QUERY_CACHE_INFO=ON -DWITH_QUERY_RESPONSE_TIME=ON -DWITH_SAFEMALLOC=AUTO -DWITH_XTRADB_STORAGE_ENGINE=ON -DWITH_LOCALES=ON -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITHOUT_TOKUDB=1 

make -j `grep processor /proc/cpuinfo | wc -l`; make install
```

### 설정변경
```
vi /opt/mysql/support-files/mysql.server
basedir=/opt/mysql
datadir=/opt/mysql/var
mysql_install_db 패스워드 초기화 후  설치 디렉터리 및 데이터  /opt/mysql/var 로 설정
/opt/mysql/scripts/mysql_install_db --basedir=/opt/mysql --datadir=/opt/mysql/var
```

### my.cnf 설정변경
```
[mysqld]
init_connect=SET collation_connection=utf8_general_ci
init_connect=SET NAMES utf8
character-set-server=utf8
collation-server=utf8_general_ci
table_open_cache=1024
max_connections=2048
max_user_connections=500
max_connect_errors=10000
wait_timeout=300
query_cache_type=1
query_cache_size=128M
query_cache_limit=5M
slow_query_log
long_query_time=3
max_allowed_packet=16M
sort_buffer_size=2M
skip-name-resolve
symbolic-links=0

[mysql]
default-character-set=utf8
```

### MariaDB 계정, 그룹 생성
```
useradd -M -s /bin/false mysql

MariaDB 설치된 Mariadb 경로 권한 설정
chown mysql:mysql /opt/mysql -R
chmod 755 /opt/mysql -R
```

### Apache 2.4.39 설치
```
mkdir /root/src/
cd /root/src
wget https://archive.apache.org/dist/httpd/httpd-2.4.39.tar.gz
tar xvfz httpd-2.4.39.tar.gz
cd /root/src/httpd-2.4.39/srclib
wget https://archive.apache.org/dist/apr/apr-1.7.0.tar.gz
tar xvfz apr-1.7.0.tar.gz
wget https://archive.apache.org/dist/apr/apr-util-1.6.1.tar.gz
tar xvfz apr-util-1.6.1.tar.gz
mv apr-1.7.0 apr; mv apr-util-1.6.1 apr-util
cd /root/src/httpd-2.4.39
```

### 컴파일 및 설치
```
./configure --prefix=/opt/apache --with-mpm=prefork --enable-headers=shared --enable-rewrite=shared --enable-mods-shared=most --with-ssl --enable-ssl --with-included-apr --with-included-apr-util --with-included-apr-iconv
make -j `grep processor /proc/cpuinfo | wc -l`; make install
```

### httpd.conf 설정변경
```
vi /opt/apache/conf/httpd.conf
<IfModule dir_module>
    DirectoryIndex index.html index.php index.htm
</IfModule>

AddType application/x-httpd-php .php .php3 .php4 .php5 .html .htm .inc
 AddType application/x-httpd-php-source .phps
```

###  PHP 7.2.18 설치
```
cd /root/src
wget https://www.php.net/distributions/php-7.2.18.tar.gz
tar -xf php-7.2.18.tar.gz
cd php-7.2.18
```
### 컴파일 및 설치
```
./configure --prefix=/opt/php --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-apxs2=/opt/apache/bin/apxs --with-curl --with-gd --with-jpeg-dir=/usr --with-freetype-dir=/usr --with-png-dir=/usr --with-xpm-dir=/usr --with-zlib --with-zlib-dir=/usr  --with-gettext --with-iconv --with-openssl-dir --with-libxml-dir=/usr/lib --with-bz2 --enable-exif --enable-ftp --enable-sockets --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-soap --enable-mbstring=all --enable-bcmath --enable-zip

make -j `grep processor /proc/cpuinfo | wc -l`; make install
```

### php.ini 수정
```
cp -f /root/src/php-7.2.18/php.ini-production /opt/php/lib/php.ini
short_open_tag = On

vi /opt/apache/htdocs/phpinfo.php
<?
phpinfo();
?>
```
