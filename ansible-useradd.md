# 앤서블 사용자 계정 생성
### Ansible 플레이북 생성
```
vi create_user.yml
---


- hosts: all
  tasks:
  - name: Create User {{ user }}
    ansible.builtin.user:
      name: "{{ user }}"
      state: present
```

###  플레이북 문법 체크
```
ansible-playbook --syntax-check create_user.yml
```

### 플레이북 실행
```
ansible-playbook create_user.yml
```
