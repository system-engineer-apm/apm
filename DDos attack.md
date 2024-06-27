# DDos 공격 방어
### ApacheBench(ab) 설치
```
yum install httpd-tools
```

### mod_evasive 설치
```
yum -y install mod_evasive
```

### mod_evasive 설정
```
vim /etc/httpd/conf.d/mod_evasive.conf
DOSLogDir           "/var/log/mod_evasive"

sudo mkdir /var/log/mod_evasive
sudo chown apache:apache /var/log/mod_evasive
sudo systemctl restart httpd
sudo tail -f /var/log/mod_evasive/dos-<IP>.log
```
### 로드 테스터 사용
ab -n 10000 -c 100 [IP 주소 혹은 도메인]
