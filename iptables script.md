# iptables script


### iptables script
```
vi iptables.sh
#!/bin/bash

function _Setting() {
    ### Chain Flush
    ${IPTABLES} -F WHITELIST
    ${IPTABLES} -F STRING
    ${IPTABLES} -F SSH
    ${IPTABLES} -F MYSQL
    ${IPTABLES} -F INPUT
    ${IPTABLES} -F FORWARD
    ${IPTABLES} -F OUTPUT
    ### Chain Delete
    ${IPTABLES} -X STRING
    ${IPTABLES} -X WHITELIST
    ${IPTABLES} -X SSH
    ${IPTABLES} -X MYSQL
    ### Chain Create
    ${IPTABLES} -N STRING
    ${IPTABLES} -N WHITELIST
    ${IPTABLES} -N SSH
    ${IPTABLES} -N MYSQL
    ### INPUT Filter
    ${IPTABLES} -A INPUT -j WHITELIST
    ${IPTABLES} -A INPUT -p tcp -m tcp --dport 80 -j STRING
    ${IPTABLES} -A INPUT -p tcp -m tcp --dport 22 -j SSH
    ${IPTABLES} -A INPUT -p tcp -m tcp --dport 3306 -j MYSQL
    ${IPTABLES} -A INPUT -p tcp -m multiport -s 0.0.0.0/0 -d $IPADDR --dport 80,443 -j ACCEPT
    ${IPTABLES} -A INPUT -p tcp -m state --state ESTABLISHED,RELATED -j ACCEPT
    ${IPTABLES} -A INPUT -p udp -m state --state ESTABLISHED,RELATED -j ACCEPT
    ${IPTABLES} -A INPUT -j DROP
    ### SSH   
    ${IPTABLES} -A SSH -s  61.43.126.225 -j ACCEPT
    ${IPTABLES} -A SSH -s ${IPADDR} -j ACCEPT
    ${IPTABLES} -A SSH -j DROP
    ### MySQL
    ${IPTABLES} -A MYSQL -s ${IPADDR} -j ACCEPT
    ${IPTABLES} -A MYSQL -j DROP
  
    ### Whitelist
    ${IPTABLES} -A WHITELIST -p all -s 127.0.0.1 -j ACCEPT
    ${IPTABLES} -A WHITELIST -p all -s ${IPADDR} -j ACCEPT
  
    ### Customer All Allow
    #${IPTABLES} -A WHITELIST -s [IP] -j ACCEPT
    #### ICMP Drop
    ${IPTABLES} -A OUTPUT -p ICMP -j DROP
    ${IPTABLES} -A OUTPUT -o lo -p tcp --dport 22 -j DROP

    ### String
    ${IPTABLES} -A STRING -m string --string "pma_user" --algo bm --to 65535 -j DROP
    ${IPTABLES} -A STRING -m string --string "wget" --algo bm --to 65535 -j DROP
    # END
    echo "iptables setting end."
}
function _VarCheck() {
    # Amazon Linux 2 확인
    if grep -q "Amazon Linux" /etc/os-release && grep -q "VERSION_ID=\"2\"" /etc/os-release; then
        OS_VER="2"
    else
        echo "This script is intended for Amazon Linux 2."
        exit 1
    fi

    # iptables 명령어 위치 확인
    IPTABLES=$(which iptables)
    if [ -z "${IPTABLES}" ]; then
        echo "Check iptables command. (which iptables)"
        exit 1
    fi

    # IP 주소 확인
    # Amazon Linux 2에서는 `ip addr` 명령어를 사용합니다.
    IPADDR=$(ip addr show | grep 'inet ' | grep -v '127.0.0.1' | awk '{print $2}' | cut -d/ -f1 | head -n 1)
    if [ -z "${IPADDR}" ]; then
        echo "Could not determine IP address."
        exit 1
    fi
}

function _Main() {
    _VarCheck
    _Setting
}
_Main```

