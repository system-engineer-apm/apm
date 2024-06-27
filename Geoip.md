# 국가별 IP 차단

### 필요한 패키지 설치
```
sudo yum groupinstall "Development Tools"
sudo yum install gcc make automake autoconf libtool
```

### CPAN 모듈 설치
```
sudo amazon-linux-extras install epel -y
sudo yum install perl-CPAN
sudo cpan install Net::CIDR::Lite
sudo yum install iptables iptables-devel
```

### xtables-addons 컴파일 및 설치
```
wget https://inai.de/files/xtables-addons/xtables-addons-3.26.tar.xz
tar -xf xtables-addons-3.26.tar.xz
cd xtables-addons-3.26
./configure
make
sudo make install
```

### GeoIP 데이터베이스 다운로드 및 빌드
```
sudo yum install perl-Text-CSV_XS -y
cd /home/ec2-user/xtables-addons-3.26/geoip
sudo ./xt_geoip_dl
sudo mkdir -p /usr/share/xt_geoip
sudo ./xt_geoip_build -D /usr/share/xt_geoip *.csv
```

### Perl cpanm 설치 및 모듈 설치
```
sudo yum install perl-App-cpanminus
sudo cpanm Net::CIDR::Lite
sudo cpanm Text::CSV_XS
```

### iptables 규칙 설정
```
sudo iptables -I INPUT -m geoip --src-cc US -j DROP
sudo iptables -I INPUT -m geoip --src-cc CN -j DROP
sudo iptables -I INPUT -m geoip ! --src-cc KR -j DROP
sudo iptables -I INPUT -s 127.0.0.1 -j ACCEPT
```
