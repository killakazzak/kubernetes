## LOAD BALANCERS
### MASTER NODE

```bash
yum install -y haproxy lsyncd keepalived
```
### Keepalived

```bash
#!/bin/bash

# Проверка на количество аргументов
if [ "$#" -ne 3 ]; then
    echo "Использование: $0 <IP_MASTER_NODE> <IP_SLAVE_NODE> <IP_VIP>"
    exit 1
fi

# Присваиваем аргументы переменным
IP_MASTER_NODE=$1
IP_SLAVE_NODE=$2
IP_VIP=$3

# Создаем файл конфигурации
cat <<EOF > /etc/keepalived/keepalived.conf
vrrp_script chk_haproxy {
    script "killall -0 haproxy"
    interval 2
}

vrrp_instance VI_1 {
    interface ens192
    state MASTER
    priority 100

    virtual_router_id 1
    unicast_src_ip $IP_MASTER_NODE
    unicast_peer {
        $IP_SLAVE_NODE
    }

    track_script {
        chk_haproxy
    }

    virtual_ipaddress {
        $IP_VIP
    }
}
EOF

echo "Конфигурация Keepalived успешно создана."
```

###  Haproxy

```bash

```

```bash
systemctl enable  --now haproxy lsyncd keepalived && systemctl is-active haproxy && systemctl is-active lsyncd && systemctl is-active keepalived
```
