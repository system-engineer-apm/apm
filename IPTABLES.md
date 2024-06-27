# IPTABLES 설치 및 설정


### iptables 설치 및 시작
```
sudo yum install iptables-services -y
sudo systemctl start iptables
sudo systemctl enable iptables
```

### 기본 체인 설정
```
sudo iptables -L INPUT
sudo iptables -L FORWARD
sudo iptables -L OUTPUT
```

###  규칙 추가하기
```
sudo iptables -A INPUT -s <차단할 IP 주소> -j DROP
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

###  규칙 저장 및 복원
```
sudo service iptables save
sudo iptables-restore < /etc/sysconfig/iptables
```

