# Modeseucirty 설치 및 설정


### Modeseucirty 설치
```
yum install mod_security
```

### OWASP CRS 설치
```
cd /etc/httpd/modsecurity.d
sudo yum install git -y
sudo git clone https://github.com/coreruleset/coreruleset.git
sudo mv coreruleset/* .
```

###  기본 규칙 파일 활성화
```
sudo cp crs-setup.conf.example crs-setup.conf
```

###  Apache와의 연동
```
vi /etc/httpd/conf.d/mod_security.conf
# 아래의 설정을 파일에 추가합니다.
<IfModule mod_security2.c>
    # ModSecurity Core Rules Set configuration
    IncludeOptional modsecurity.d/*.conf
    IncludeOptional modsecurity.d/activated_rules/*.conf
    Include modsecurity.d/rules/*.conf
</IfModule>

systemctl restart httpd
```

