##APM YUM 설치

```
###Apache 설치
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

###MariaDB(MySQL 대체) 설치
sudo yum install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation

###PHP 설치
sudo yum install php php-mysqlnd php-fpm php-json
sudo systemctl restart httpd

###PHP 테스트
Apache와 PHP가 제대로 작동하는지 확인하기 위해, 간단한 PHP 파일을 생성합니다
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
