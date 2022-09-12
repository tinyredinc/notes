# DISK TOOL

Rufus - <https://rufus.ie/en/>  
Create bootable USB drives the easy way

# UBUNTU
Download Link <https://ubuntu.com/download/server>

- Ubuntu Server 22.04.1 LTS - EOL April 2027  
- Ubuntu Server 20.04 LTS - EOL April 2025

Installation Guide - <https://ubuntu.com/tutorials/install-ubuntu-server>

## PACKAGE MGMT

```
# list packages(list packages based on package names)
sudo apt list mysql-server-8.0
sudo apt list 'mysql*'
sudo apt list --upgradable
sudo apt list --installed
sudo apt list --installed '*python*'

# search packages(search in package descriptions)
apt search 'mysql database server'

# install package
sudo apt install {app_name}

$ remove packages
sudo apt remove {app_name}

# update packages(patch to versions)
sudo apt update

# upgrade packages(get new versions)
sudo apt upgrade
sudo apt upgrade {app_name}

# edit source
sudo apt edit-sources
```

## USER CONFIG
```
sudo adduser red
sudo passwd red
sudo addgroup dev
sudo adduser red dev
sudo visudo
...
# Allow members of group dev to sudo without password
%dev    ALL=(ALL) NOPASSWD: ALL
...
```
* more refers to [linux_openssh.md](linux_openssh.md)

## Service Mgmt - systemctl
```
systemctl list-units --type=service
sudo systemctl status application.service

sudo systemctl start application.service
sudo systemctl stop application.service
sudo systemctl restart application.service

sudo systemctl enable application.service
sudo systemctl disable application.service

sudo systemctl edit --full application.service
```

# CENTOS
Download Link <https://www.centos.org/download/>
- CentOS Linux 7 EOL: 2024-06-30
- CentOS Stream 8 EOL: 2024-05-31
- CentOS Stream 9 EOL: 2027-05-31  

CentOS Linux(7) is the downstream rebuild of Red Hat Enterprise Linux (RHEL)  
CentOS Stream(8,9) is the upstream, public development branch for Red Hat Enterprise Linux (RHEL) 