# Memory Lane

  * [Memory Lane](#memory-lane)
  * [About](#About)
  * [iptables](#iptables)
  * [general Linux tidbits](#linux)
  * [scripting](#scripting)
  * [network](#network)
  * [lxc](#lxc)
  * [docker](#docker)
  * [heroku](#heroku)
  * [notifications](#notifications)  
  * [git](#git)
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

## scripting

Bash profiling

At the top of the script:
```shell
N=`date +%s%N`; export PS4='+[$(((`date +%s%N`-$N)/1000000))ms][${BASH_SOURCE}:${LINENO}]: ${FUNCNAME[0]:+${FUNCNAME[0]}(): }';
exec 19>$HOME/logfile
BASH_XTRACEFD=19
set -x
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

## Docker

List docs from all docker containers

```shell
	sudo sysdig -pc -cspy_logs
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

## git

Archive untracked files

```
git ls-files --others --exclude-standard -z |\

xargs -0 tar rvf ~/backup-untracked.zip
```

find in all commits

```

git rev-list –all | xargs git grep -F ‘font-size: 52 px;’

F3022…9e12:HtmlTemplate/style.css: font-size: 52 px;

E9211…8244:RR.Web/Content/style/style.css: font-size: 52 px;
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

GitHub Actions, set up Maven for local runs using act

```yaml
    - name: Download Maven
      run: |
        curl -sL https://www-eu.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip -o maven.zip
        apt-get update
        apt-get -y install unzip
        unzip -d /usr/share maven.zip
        rm maven.zip
        ln -s /usr/share/apache-maven-3.6.3/bin/mvn /usr/bin/mvn
        echo "M2_HOME=/usr/share/apache-maven-3.6.3" | tee -a /etc/environment
```
