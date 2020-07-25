# Memory Lane

* [Memory Lane](#memory-lane)
  * [About](#About)
  * [iptables](#iptables)
  * [general Linux tidbits](#linux)
  * [network](#network)
  * [lxc](#lxc)
  * [heroku](#heroku)
  * [notifications](#notifications)
  * [misc](#uncategorised)

## About

Storage of snippets that I use too frequently to google every time, but too infrequently to actually remember the damn thing.

## iptables

Simple NAT masquerade

```shell
    iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE 
```

## network

Replace default gateway

```shell
    ip route replace default via 1.2.3.4
```

Swap mac address

```shell
    ifconfig eth0 hw ether $MAC
```

Port Scan

```shell
    nc -v -n -z -wl $TARGET $IP_FROM-$IP_TO
```

Easy HTTP server

```shell
    python -m SimpleHTTPServer 1337
```

## linux

Increase inotify watcher limits

```shell
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

## lxc

Attach host folder

```shell
    lxc config device add $CONTAINER_NAME $MOUNT_NAME disk source=/var/www/ path=/var/www
```

Limit container resources

```shell
    lxc config set $CONTAINER_NAME limits.cpu 2
    lxc config set $CONTAINER_NAME limits.memory 2024MB
    lxc config get $CONTAINER_NAME limits.memory.swap 
    lxc config set $CONTAINER_NAME limits.memory.swap 1024MB
    lxc config set $CONTAINER_NAME limits.memory.swap.priority 0
```

Forward a port from lxc to host

```shell
    lxc config device add $CONTAINER_NAME $DEVICE_NAME proxy listen=tcp:0.0.0.0:1337 connect=tcp:127.0.0.1:1337
```

## heroku

Cancel a stuck build
```shell
heroku plugins:install heroku-builds #first time
heroku builds:cancel $BUILD_UUID -a $APP_ID
```

## notifications

Curl telegram message

```shell
	curl -X POST \
	     -H 'Content-Type: application/json' \
	     -d '{"chat_id": "$CHATID", "text": "Message Text", "disable_notification": true}' \
	     https://api.telegram.org/bot$BOT_TOKEN/sendMessage
```

## uncategorised 

Substitute shell vars in a file (Templating)

```shell
envsubst < /etc/radsecproxy.conf.tmpl > /etc/radsecproxy.conf
```

Make DELL PowerEdge servers shut up

```shell
# enter manual fan control mode
ipmitool -I lanplus -H $IDRAC_HOST -U root -P $IDRAC_PW raw 0x30 0x30 0x01 0x00 
# make the noise go away, shhhh...
ipmitool -I lanplus -H $IDRAC_HOST -U root -P $IDRAC_PW raw 0x30 0x30 0x02 0xff 0x09
```
