# Memory Lane

- [Memory Lane](#memory-lane)
    - [About](#About)
    - [iptables](#iptables)
    - [lxc](#lxc)

## About
Storage of snippets that I use too frequently to google every time, but too infrequently to actually remember the damn thing.

## iptables

Simple NAT masquerade
```
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE 
```

## lxc

Attach host folder
```
  lxc config device add $CONTAINER_NAME $MOUNT_NAME disk source=/var/www/ path=/var/www
```
