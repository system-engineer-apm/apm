# SSL 설치 및 설정

### Certbot써트봇과 Python2 기반의 Apache 플러그인을 설치
```
yum install certbot python2-certbot-apache
```

### Apache 설정 검증
```
httpd -t
systemctl restart httpd
```

### SSL 인증서 발급 및 적용
```
sudo certbot --apache
```

### SSL 인증서 확인
```
openssl s_client -connect 도메인 주소:443 -showcerts
```
