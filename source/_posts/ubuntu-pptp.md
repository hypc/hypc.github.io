---
title: Ubuntu搭建PPTP服务
date: 2018-07-11 13:09:11
tags: [ubuntu, pptp, vpn]
---

```bash
apt-get update && apt-get install -y pptpd

cat <<EOF>> /etc/pptpd.conf
localip     192.168.17.1
remoteip    192.168.17.100-199
EOF

cat <<EOF>> /etc/ppp/pptpd-options
ms-dns  192.168.17.1
ms-dns  8.8.8.8
EOF

cat <<EOF>> /etc/ppp/chap-secrets
username    *    password    *
EOF

systemctl enable pptpd
systemctl restart pptpd

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
sysctl -p
iptables -t nat -A POSTROUTING -s 192.168.17.0/24 -o eth0 -j MASQUERADE
```
