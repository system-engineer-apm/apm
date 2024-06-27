# 앤서블 패스워드 설정


### SHA-512 해시 생성
```
[root@ip-172-31-33-88 my-ansible]# python
Python 2.7.18 (default, Dec 18 2023, 22:08:43)
[GCC 7.3.1 20180712 (Red Hat 7.3.1-17)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import crypt
>>> password = "qwe123@@"  # 비밀번호를 여기에 입력하세요
>>> salt = crypt.mksalt(crypt.METHOD_SHA512)  # SHA-512 솔트 생성
>>> hash = crypt.crypt(password, salt)
>>> print(hash)
$6$YV5oQ846NAhIyKcp$ZlXCPtJd9izJiBYV6CVf/sI8ufxGAg4LsXltH7KJXJnd7mySf9iOf42lkr96.8Sd2Gz/PAkpdhPQRLf0XvKKt0```
```

### Ansible 플레이북 생성
```
vi update_password.yml
- hosts: servers
  vars:
    user_password_hash: "$6$YV5oQ846NAhIyKcp$ZlXCPtJd9izJiBYV6CVf/sI8ufxGAg4LsXltH7KJXJnd7mySf9iOf42lkr96.8Sd2Gz/PAkpdhPQRLf0XvKKt0"
  tasks:
    - name: Ensure user is present with a password
      user:
        name: "root"
        password: "{{ user_password_hash }}"
        state: present
```

###  플레이북 문법 체크
```
ansible-playbook --syntax-check update_password.yml
```

### 플레이북 실행
```
ansible-playbook update_password.yml
```
