## LOAD BALANCERS
### MASTER NODE

```bash
yum install -y haproxy lsyncd keepalived
```
### Keepalived

```bash
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
    unicast_src_ip {IP MASTER NODE}
    unicast_peer {
        {IP SLAVE NODE}
    }

    track_script {
        chk_haproxy
    }

        virtual_ipaddress {
        {IP VIP}
    }
}
EOF

###  Haproxy

```bash

```

```bash
systemctl enable  --now haproxy lsyncd keepalived && systemctl is-active haproxy && systemctl is-active lsyncd && systemctl is-active keepalived
```
