# Apache 보안 설정
```
echo '<html><body><h1>Hello, World!</h1></body></html>' | sudo tee /var/www/html/index.html
curl ifconfig.me

vi /etc/httpd/conf/httpd.conf
ServerTokens Full # Full: 모든 컴파일 옵션 정보를 포함하여 노출
ServerSignature On # 웹 브라우저에 정보 노출 활성화

sudo systemctl restart httpd
```
