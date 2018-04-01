## Install dnsmasq
```
yum install dnsmasq

vi /etc/dnsmasq.d/openshift-cluster.conf
-- Add below lines
strict-order
listen-address=192.168.3.53,127.0.0.1
address=/feng.com/192.168.3.53


vi /etc/resolv.conf
-- Add below lines
nameserver 127.0.0.1

```

## Add node ips
```
vi /etc/hosts
-- Add below lines
192.168.3.58 feng.com
192.168.3.59 node.feng.com

```

## Restart dnsmasq
```
systemctl restart dnsmasq
```

## Node config
```
vi /etc/resolv.conf
-- Add below lines, change with yours dns server ip
nameserver 192.168.3.53
```

## Firewalld config
```
systemctl start firewalld
firewall-cmd --add-service=dns --permanent
firewall-cmd --reload
```


## Utility
```
-- How to stop NetworkManager updating /etc/resolv.conf
vi /etc/NetworkManager/NetworkManager.conf

-- And add this to the [main] section
dns=none
```