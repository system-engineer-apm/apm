##APM Source 설치
## EPEL 저장소를 설치
```
sudo amazon-linux-extras install epel -y
```

## APM 컴파일을 위한 의존성 패키지 설치
```
sudo yum groupinstall "Development Tools" -y
yum -y install make gcc gcc-c++ ncurses-devel libevent openssl openssl-devel gnutls-devel libxml2 libxml2-devel bison gmp gmp-devel bzip2-devel curl-devel libjpeg-devel libXpm-devel
yum -y install libtool expat-devel pcre-devel apr-devel apr-util libzip pcre-devel expat-devel apr-util-devel libdb-devel libpng-devel freetype-devel cmake
```

##MariaDB 10.3.39 설치
```
mkdir -p /root/src
cd /root/src
wget https://archive.mariadb.org/mariadb-10.3.39/source/mariadb-10.3.39.tar.gz
tar xvfz mariadb-10.3.39.tar.gz
cd mariadb-10.3.39
```
##컴파일 및 설치
```
wget https://github.com/Kitware/CMake/releases/download/v3.22.1/cmake-3.22.1.tar.gz
tar -zxvf cmake-3.22.1.tar.gz
cd cmake-3.22.1
./bootstrap
make
make install

vi ~/.bash_profile
PATH=$PATH:/root/src/cmake-3.22.1/bin

cmake -DCMAKE_INSTALL_PREFIX=/opt/mysql -DMYSQL_DATADIR=/opt/mysql/var -DMYSQL_UNIX_ADDR=/tmp/mysql.sock -DENABLED_LOCAL_INFILE=ON -DWITH_SSL=system -DWITH_ZLIB=system -DWITH_JEMALLOC=no -DWITH_ARIA_STORAGE_ENGINE=ON -DUSE_ARIA_FOR_TMP_TABLES=ON -DWITH_PARTITION_STORAGE_ENGINE=ON -DWITH_PERFSCHEMA_STORAGE_ENGINE=ON -DWITH_QUERY_CACHE_INFO=ON -DWITH_QUERY_RESPONSE_TIME=ON -DWITH_SAFEMALLOC=AUTO -DWITH_XTRADB_STORAGE_ENGINE=ON -DWITH_LOCALES=ON -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITHOUT_TOKUDB=1 

make -j `grep processor /proc/cpuinfo | wc -l`; make install
```

## 설정변경
```
vi /opt/mysql/support-files/mysql.server
basedir=/opt/mysql
datadir=/opt/mysql/var
mysql_install_db 패스워드 초기화 후  설치 디렉터리 및 데이터  /opt/mysql/var 로 설정
/opt/mysql/scripts/mysql_install_db --basedir=/opt/mysql --datadir=/opt/mysql/var
```

## my.cnf 설정변경
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
