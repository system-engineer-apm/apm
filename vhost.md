# 아파치 vhost(가상호스트) 설정


```
# ServerName 에는 서비스할 도메인을 적어주고
# ServerAlias 를 통해 추가적으로 별칭을 지정

cd /etc/httpd/conf.d
vi vhost.conf

<VirtualHost *:80>
        DocumentRoot "/var/www/html"
        ServerName apmaws.duckdns.org # 도메인 주소
        ServerAlias www.apmaws.duckdns.org
        ErrorLog "logs/apmaws.duckdns.org-error_log" # 에러로그
        CustomLog "logs/apmaws.duckdns.org-access_log" common # 접속로그
<Directory "/var/www/html">
    Options indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
</VirtualHost>

<VirtualHost *:80>
        DocumentRoot "/var/www/html2"
        ServerName apmaws2.duckdns.org # 도메인 주소
        ServerAlias www.apmaws2.duckdns.org
        ErrorLog "logs/apmaws2.duckdns.org-error_log" # 에러로그
        CustomLog "logs/apmaws2.duckdns.org-access_log" common # 접속로그
<Directory "/var/www/html2">
    Options indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
</VirtualHost>
```

### 도메인의 DNS 설정 확인
```
dig +short [도메인 주소]
```
