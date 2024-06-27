# Apache 보안 설정
```
echo '<html><body><h1>Hello, World!</h1></body></html>' | sudo tee /var/www/html/index.html
curl ifconfig.me

vi /etc/httpd/conf/httpd.conf
ServerTokens Prod # 웹 서버 정보 노출 설정 최소화
ServerSignature off # 웹 브라우저에 정보 노출 비활성화

sudo systemctl restart httpd
```
