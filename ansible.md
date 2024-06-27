# 앤서블 환경 구성


### SSH 설정 변경
```
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
# SSH 서비스 재시작
systemctl restart sshd
```

### Ansible 설치
```
sudo amazon-linux-extras install ansible2 -y
```

### 앤서블 환경 설정 파일 생성
```
mkdir -p /root/my-ansible
vi ansible.cfg
[defaults]
inventory = /root/my-ansible/inventory
remote_user = root
ask_pass = false

[privileges_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false
```

### IP를 이용한 인벤토리 파일 생성
```
vi inventory
[servers]
# 서버 IP 주소
```

### SSH 키 생성 및 복사
```
# SSH 키 생성
ssh-keygen -t rsa -b 2048

# SSH 키 복사
ssh-copy-id root@[IP]
```

### Ping 테스트
```
ansible servers -m ping
```
