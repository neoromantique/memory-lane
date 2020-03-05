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
Limit container resources 
```
    lxc config set $CONTAINER_NAME limits.cpu 2
    lxc config set $CONTAINER_NAME limits.memory 2024MB
    lxc config get $CONTAINER_NAME limits.memory.swap 
    lxc config set $CONTAINER_NAME limits.memory.swap 1024MB
    lxc config set $CONTAINER_NAME limits.memory.swap.priority 0
```
Forward a port from lxc to host
```
    lxc config device add $CONTAINER_NAME $DEVICE_NAME proxy listen=tcp:0.0.0.0:1337 connect=tcp:127.0.0.1:1337
```
