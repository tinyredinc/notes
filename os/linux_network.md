# STATIC IP

## UBUNTU
- \# check network adpaters  
- sudo ip a
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether f4:4d:30:65:dd:28 brd ff:ff:ff:ff:ff:ff
    altname enp0s31f6
    inet 10.10.10.20/24 brd 10.10.10.255 scope global eno1
       valid_lft forever preferred_lft forever
3: wlp1s0: <BROADCAST,MULTICAST> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether a0:c5:89:24:4a:69 brd ff:ff:ff:ff:ff:ff
```
- \# get config file list for netplan  
- sudo ls -la /etc/netplan/
```
drwxr-xr-x   2 root root 4096 Aug 25 06:12 .
drwxr-xr-x 100 root root 4096 Aug 30 15:17 ..
-rw-------   1 root root  184 Aug 24 06:56 00-installer-config-wifi.yaml.bak
-rw-r--r--   1 root root  287 Aug 25 06:12 eno1.yaml
```
- \# create or edit netplan config file
- sudo vim /etc/netplan/eno1.yaml
```
network:
    version: 2
    renderer: networkd
    ethernets:
        eno1:
            addresses:
                - 10.10.10.20/24
            nameservers:
                addresses: [8.8.8.8, 8.8.4.4]
            routes:
                - to: default
                  via: 10.10.10.1
```
- \# apply the change to netplan
- sudo netplan try apply
```
Do you want to keep these settings?

Press ENTER before the timeout to accept the new configuration

Changes will revert in 101 seconds
Configuration accepted.
```
- \# turn off wifi adapter since wired connection is prefered
- sudo ip link set wlp1s0 down